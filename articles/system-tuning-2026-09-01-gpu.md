# 約3%の性能と引き換えに、RTX 3080の消費電力を約100 W下げる

[← ケース・冷却編](./system-tuning-2026-09-01-case-cooling.md) / [索引](./system-tuning-2026-09-01-index.md) / [次: CPU・BIOS編 →](./system-tuning-2026-09-01-cpu-bios.md)

ケース側の冷却をかなり整理できると、次に目立ってきたのがRTX 3080の発熱でした。

Rearを100%まで回してもほとんど温度は変わらず、Bottomは止めても差がほぼない。

だったらケースファンをさらに追い込むより、ケースへ入ってくる熱そのものを減らした方が効きそうです。

そこでGPUのV/F Curve、電圧と動作クロックの関係を調整することにしました。

ただ、その前にひとつ別の問題がありました。

FF14のスコアが以前の18,000台へ戻らなくなっていたからです。

## スコアが戻らない。最初は冷却を疑った

再起動してもFF14 Scoreは17,674付近のままでした。

GPU ClockもPowerも落ちていません。GPU最大温度は74℃で、継続的なthermal throttlingもありませんでした。

メモリ使用量を減らしても変わらず、再起動しても再現します。

で、冷却以外を疑うことにしました。

Windows HypervisorとVBSを一時的に無効化して同じ条件でA/Bすると、結果はこうなりました。

| 条件 | Score | Average FPS |
|---|---:|---:|
| Hypervisor ON | 17,674 | 123.7877 |
| Hypervisor OFF | 18,128 | 126.0923 |
| 差 | +454 / +2.57% | +2.3046 / +1.86% |

Hypervisorを止めると18,000台へ戻りました。

この差はHypervisor / VBSの固定的なオーバーヘッドで説明できる可能性が高いと思います。

ただ、速くなるからOFFを採用、とはしませんでした。

このPCではWSLや仮想化を使います。セキュリティ機能も含めて残す価値があります。

なのでHypervisorはONへ戻しました。

## まず1905 MHz / 825 mVを試した

RTX 3080の発熱を大きく減らすため、最初は1900 MHz / 825 mVを狙いました。

実際の保存値はGPUのclock binに合わせて1905 MHzです。825 mV以上のV/F Curveを1905 MHzへ水平化しました。

FF14を開始すると、途中でDirectX Errorになりました。

Windows Event Logには、

- `nvlddmkm` Event ID 153
- LiveKernelEvent 141が2件

が残りました。

公式Scoreも生成されていません。

V/F変更直後の最初の負荷でGPUドライバの停止やリセットと整合するイベントが出ているので、825 mVは攻めすぎと判断しました。

無理に原因を細かく追うより、まず25 mV戻してどうなるかを見る方が早い。

ということで850 mVへ戻しました。

## 850 mVまで戻すと、短いベンチは通った

clockは1905 MHzのまま、V/F Curveの水平化を始める点だけ825 mVから850 mVへ変更しました。

この設定ではFF14を完走しました。

従来設定との比較はこうなりました。

| Metric | 従来GPU設定 | 1905 MHz / 850 mV | 差 |
|---|---:|---:|---:|
| Score | 17,674 | 17,082 | -592 / -3.35% |
| GPU voltage平均 | 1.065 V | 0.867 V | -0.198 V |
| GPU clock平均 | 2,118 MHz | 1,916 MHz | -202 MHz |
| GPU power平均 | 350.7 W | 252.8 W | -97.9 W |
| GPU温度平均 | 66.8℃ | 58.9℃ | -7.9℃ |
| Hotspot平均 | 83.4℃ | 73.6℃ | -9.9℃ |

Scoreは約3.35%下がりました。

一方で平均GPU Powerは約98 W下がり、GPU温度は約8℃、Hotspotは約10℃下がっています。

これは「性能をほぼ落とさず省電力化できた」という結果ではありません。

**Scoreが約3.35%下がる一方で、平均GPU Powerが約98 W、GPU温度が約8℃、Hotspotが約10℃下がった**設定です。

今の使い方なら、この差なら850 mV側を使ってよさそうだと思いました。

ケース側ではRearを約700 RPM増やしてもほとんど冷えませんでした。

それに対してGPU側では、平均Powerそのものが約100 W下がっています。

ケースファンをさらに回すより、こちらを先に詰めることにしました。

## ただ、短いベンチが通っただけだった

825 mVより850 mVの方が明らかに良さそうです。

ただ、ここまでの冷却調整で短時間の結果だけを見る危なさも分かっていました。

そこで1905 MHz / 850 mVのまま、FF14を長時間ループさせました。

結果は59分03.988秒でBugCheck 0x50。

予期しない再起動です。

さらに、その約12分前にはWHEA Event ID 2が1件出ていました。

WHEAは、CPUやPCIeなどのハードウェアがWindowsへ通知するエラー記録です。今回はRaw CPERを読むと、`corrected processor cache / instruction-execution / level 0 / local APIC ID 2`でした。

クラッシュ直前の温度は、

- CPU 65℃
- GPU 63℃
- Hotspot 78.5℃
- Liquid 43℃

です。

thermal throttlingもpower limitもありません。

`nvlddmkm`やLiveKernelEvent 141も出ていません。

825 mVで失敗したときとは落ち方が違います。

## GPUを触っていたら、CPU側の問題が見えてきた

WinDbgでは`cam_helper.exe`がmutexを作成していたときのkernel stackが残っていました。

ただ、stack上にCAMのthird-party kernel moduleが原因だと示す証拠はありません。

なので「CAMが原因だった」とはしませんでした。

一方で、クラッシュ前にCPU/cache系のcorrected WHEAが出ています。

CPU/cache、RAM、memory controller、あるいは別のcorruption要因かもしれません。

ここで確実に言えるのは、GPU温度やGPU driver resetが直接見えていた825 mV時とは違う、というところまでです。

ここはAIエージェントを使っていなければ、たぶん「長時間だと落ちた」で終わっていたと思います。今回はWindows Event Log、WHEAのRaw CPER、WinDbg、GPUドライバのイベント、温度ログを同じ時間帯で突き合わせて、825 mV時とは別の落ち方だと整理できました。

原因そのものはまだ断定できません。ただ、GPUの850 mVをすぐ不採用にするのではなく、次はCPU側の動作条件を見てみようと判断できました。

そしてもうひとつ。

**短いFF14を1回完走しても、1時間後に安定しているとは限らない。**

この試験で、それがかなりはっきりしました。

## 850 mVは残し、次はCPUの最高倍率を100 MHz下げる

GPU 1905 MHz / 850 mV自体は、825 mVと違って短期ではGPU driver errorを出していません。

平均Powerも約253 Wまで下がっています。

なのでGPU側は850 mVを候補として残しつつ、次はCPUのTurbo Ratioを全部100 MHzずつ下げて、長時間試験でどう変わるかを見ることにしました。

ここから作業はGPUチューニングからCPU BIOSチューニングへ移っていきます。

最初はGPUを約100 W軽くする話だったのに、長時間回したことでCPUの動作条件まで見ることになりました。

でも、こういう順番だったからこそ、GPUの失敗とCPU側のWHEAを同じものとして扱わずに済んだのかなと思います。

次は、5年前から使っていたCore i9-10900KのTurbo Ratioと電圧を、もう一度最初から調べ直します。

---

## 主な参照記録

- `experiments/2026-08-30-ffxiv-post-reboot-hypervisor-baseline/`
- `experiments/2026-08-30-ffxiv-hypervisor-off-ab/`
- `experiments/2026-08-30-ffxiv-gpu-vf-1905mhz-825mv-baseline/`
- `experiments/2026-08-30-ffxiv-gpu-vf-1905mhz-850mv-baseline/`
- `experiments/2026-08-30-ffxiv-gpu-vf-1905mhz-850mv-long-duration/`

[← ケース・冷却編](./system-tuning-2026-09-01-case-cooling.md) / [索引](./system-tuning-2026-09-01-index.md) / [次: CPU・BIOS編 →](./system-tuning-2026-09-01-cpu-bios.md)
