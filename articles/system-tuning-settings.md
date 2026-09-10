# このPCのBIOS・アプリ設定

CPUの倍率と電圧、GPUの低電圧化、ファンの静音化と調整を続けてきたので、いま使っている設定を一か所にまとめました。Core i9-10900KとRTX 3080を搭載した、このPCでの設定です。

## 設定を探す

| 知りたい設定 | 設定する場所 |
| --- | --- |
| [CPUの倍率・Ring](#cpu-ratio) | ASUS BIOS |
| [CPUの電圧・電力上限](#cpu-voltage) | ASUS BIOS |
| [Turbo・省電力機能](#cpu-features) | ASUS BIOS |
| [Windowsの電源プラン](#windows-power) | Windows |
| [メモリ・XMP](#memory) | ASUS BIOS |
| [GPUの電圧・クロック](#gpu-vf) | ASUS GPU Tweak III |
| [GPUファン](#gpu-fan) | ASUS GPU Tweak III |
| [前面・背面・底面のケースファン](#case-fan) | AI Suite 3 / Fan Xpert 4 |
| [ラジエーターファン・ポンプ](#kraken) | NZXT CAM |
| [DLSS・垂直同期・普段のFPS](#game-display) | NVIDIA App / FF14 |

表には、設定画面や保存した設定で使われている項目名も添えています。部品構成は[現在使っているPC](../docs/current-pc.md)、試した順番や判断の経緯は[自作PCチューニング記録](./system-tuning-2026-09-01-index.md)を参照してください。

<a id="cpu-ratio"></a>

## CPUの倍率・Ring

マザーボードはASUS TUF GAMING Z490-PLUS (WI-FI)、CPUはCore i9-10900Kです。`CPU Core Ratio`を`By Core Usage`にして、使用コア数ごとに倍率を変えています。

| 使用コア数 | 上限倍率 |
| --- | ---: |
| 1コア | 52 |
| 2コア | 51 |
| 3コア | 50 |
| 4コア | 49 |
| 5コア | 48 |
| 6～9コア | 47 |
| 10コア | 40 |

少数コアの高倍率を残し、使うコア数が増えたところで倍率を下げています。上の表は、1～2コア52倍・3～4コア51倍・5～6コア50倍での調整を終えたあと、普段の動きを見やすくするために細かく分けた配分です。ブラウザ性能はほぼ同じだったので、いまはこの設定で様子を見ています。[倍率を分け直した経緯](./system-tuning-cpu-ratio-reconsideration.md)

<details>
<summary>BIOSの入力値：Turbo Ratio Cores / Turbo Ratio Limit</summary>

| 行 | Turbo Ratio Cores | Turbo Ratio Limit |
| --- | ---: | ---: |
| 0 | 1 | 52 |
| 1 | 2 | 51 |
| 2 | 3 | 50 |
| 3 | 4 | 49 |
| 4 | 5 | 48 |
| 5 | 6 | 47 |
| 6 | 9 | 47 |
| 7 | 10 | 40 |

</details>

| 項目 | 設定値 |
| --- | --- |
| BCLK Frequency | 100.0000 MHz |
| AVX Instruction Core Ratio Negative Offset | User Specify、Value 0 |
| Min. CPU Cache Ratio | 40 |
| Max CPU Cache Ratio | 40 |
| Ring Down Bin | Disabled |

上位倍率を残した理由と普段の動作は[CPU検証の締めくくり](./system-tuning-cpu-validation-complete.md)、7～9コアを47倍に決めた比較は[ブラウザでの倍率調整](./system-tuning-2026-09-07-browser-ratio-balance.md)にまとめています。

<a id="cpu-voltage"></a>

## CPUの電圧・電力上限

CPU電圧はAutoを使い、AC Load Lineを0.300 mΩ、V/F Point 7を−80 mVにしています。途中では固定電圧やほかのV/F Pointも試しましたが、ここに落ち着きました。

| 項目 | 設定値 |
| --- | --- |
| CPU Core/Cache Voltage | Auto |
| SVID Behavior | Typical Scenario |
| CPU Load-line Calibration | Level 4:Recommended for OC |
| Synch ACDC Loadline with VRM Loadline | Enabled |
| IA AC Load Line | 0.30（0.300 mΩ） |
| IA DC Load Line | Auto |
| V/F Point 7 Offset | Offset Mode Sign 7：−、Offset：0.080 V |
| V/F Point 1～6・8 Offset | Auto |

電力上限は、以前試していた90 Wから4095 Wへ戻しています。いまは電力上限で抑える設定にはしていません。

| 項目 | 設定値 |
| --- | --- |
| ASUS MultiCore Enhancement | Disabled – Enforce All limits |
| Long Duration Package Power Limit（PL1） | 4095 W |
| Short Duration Package Power Limit（PL2） | 4095 W |
| Package Power Time Window | Auto |
| CPU Core/Cache Current Limit Max. | 245.00 A |
| CPU Current Capability | 140% |

AC Load LineとV/F Pointを見直した経緯は[全コア40倍からのやり直し](./system-tuning-2026-09-06-ffxiv-long-run.md)、この電圧設定で調整を一区切りにした経緯は[CPU検証の締めくくり](./system-tuning-cpu-validation-complete.md)に記録しています。その後の倍率の再検討でも、電圧設定は引き継いでいます。

<a id="cpu-features"></a>

## Turbo・省電力機能

| 項目 | 設定値 |
| --- | --- |
| Intel(R) SpeedStep(tm) | Enabled |
| Intel(R) Speed Shift Technology | Enabled |
| Intel(R) Turbo Boost Max Technology 3.0 | Enabled |
| Turbo Mode | Enabled |
| TVB Voltage Optimizations | Enabled |
| Overclocking TVB | Disabled |
| Dual Tau Boost | Disabled |
| CPU C-states | Auto |
| Thermal Monitor | Enabled |
| Power-saving & Performance Mode | Performance mode |

IntelとWindowsが少数コアの性能を活かす仕組みも踏まえて、2コアの52倍を残しました。その考え方は[CPU記事のIntelとWindowsについての部分](./system-tuning-cpu-validation-complete.md)で触れています。

<a id="windows-power"></a>

## Windowsの電源プラン

Windowsの電源プランは`バランス`です。

<a id="memory"></a>

## メモリ・XMP

G.Skill F4-4000C15D-16GTZRを8 GiB×2枚で使っています。メモリはXMP IのDDR4-4000、15-16-16-36、1.50 Vです。

| 項目 | 設定値 |
| --- | --- |
| Ai Overclock Tuner | XMP I |
| XMP | XMP DDR4-4000 15-16-16-36-1.50V |
| DRAM Frequency | DDR4-4000MHz |
| DRAM CAS# Latency | 15 |
| DRAM RAS# to CAS# Delay | 16 |
| DRAM RAS# ACT Time | 36 |
| DRAM Command Rate | Auto |
| DRAM Voltage | 1.50000 V |
| CPU VCCIO Voltage | Auto |
| CPU System Agent Voltage | Auto |

<a id="gpu-vf"></a>

## GPUの電圧・クロック

ASUS ROG-STRIX-RTX3080-O10G-GAMINGを、ASUS GPU Tweak IIIの`VF750mV`プロファイルで使っています。

| 項目 | 設定値 |
| --- | --- |
| V/Fカーブの750 mV点 | 1620 MHz |
| 750 mVより右側の点 | 1620 MHzで水平に揃える |
| メモリクロック（実効表示） | 19,002 MHz |
| Power Target | 121% |
| GPU Temp Target | 91℃ |

GPUの電力はV/Fカーブ側で下げています。850 mVから800 mV、750 mVと試し、DLSS常時適用も組み合わせると、FF14ベンチのGPU平均電力は200 W弱になりました。普段のゲームで動きや見え方を確認したあと、約8時間20分のループ試験まで終えています。

設定を詰めた過程と測定結果は[RTX 3080の750 mV化とDLSSの記事](./system-tuning-gpu-750mv-dlss.md)にまとめています。

<a id="gpu-fan"></a>

## GPUファン

GPU Tweak IIIで、左右のファンを扱う`Sides`と中央の`Center`を同じカーブにしています。

| GPU温度 | ファン指定値 |
| --- | ---: |
| 30℃ | 30% |
| 60℃ | 50% |
| 70℃ | 82% |
| 85℃ | 100% |

<a id="case-fan"></a>

## 前面・背面・底面のケースファン

ケースファンはAI Suite 3のFan Xpert 4で設定しています。前面は140 mmファン3基の吸気、背面は1基の排気です。

| 位置 | Fan Xpertの表示 / 接続先 | 制御方式 | 温度ソース |
| --- | --- | --- | --- |
| 背面 | Chassis fan 1 / CHA_FAN1 | PWM | CPUとGPUの高い方 |
| 前面3基 | Chassis fan 2 / CHA_FAN2 | PWM | GPU |
| 底面 | Chassis fan 3 / CHA_FAN3 | DC、停止 | GPU |

前面3基は分配接続しているので、同じ設定で回ります。

| 温度 | 背面 | 前面 |
| --- | ---: | ---: |
| 30℃ | 20% | 20% |
| 60℃ | 40% | 40% |
| 80℃ | 約59% | 60% |

保存された設定には、この3点に加えて80℃・100%の点もあります。

背面と前面は、`ファン回転数の上昇`を12秒、`ファン回転数の下降`を25秒にしています。底面はDC制御のままカーブの全点を0%にして止めています。

待機中の音を下げるため、低温側を30℃・20%にしました。底面ファンを止めるまでの比較や、ファンを下げたあとの負荷確認は[冷却と静音化のまとめ](./system-tuning-cooling-after-undervolt.md)で読めます。

<a id="kraken"></a>

## ラジエーターファン・ポンプ

NZXT CAMの`冷却 → Kraken Plus`で設定しています。360 mmラジエーターはケース天面の排気で、`Fan`と`Pump`の温度ソースはどちらも`冷却水`です。

### Fan：カスタム

| 冷却水温 | ファン指定値 |
| --- | ---: |
| 20～35℃ | 35% |
| 40℃ | 65% |
| 45℃ | 70% |
| 50℃以上 | 100% |

GPU連動ではCPUだけに負荷がかかったときに困ります。冷却水連動でGPUの負荷にも足りていたので、この制御を使っています。低温側は以前の60%から35%へ下げました。

### Pump：サイレント

ポンプはCAM標準の`サイレント`プリセットをそのまま使っています。

| 冷却水温 | ポンプ指定値 |
| --- | ---: |
| 20～35℃ | 60% |
| 40℃ | 70% |
| 45℃ | 80% |
| 50℃ | 90% |
| 55℃以上 | 100% |

水温を見て制御するようにした経緯は[ケース・冷却編](./system-tuning-2026-09-01-case-cooling.md)、低温側の回転数を下げた話は[冷却と静音化のまとめ](./system-tuning-cooling-after-undervolt.md)にあります。

<a id="game-display"></a>

## DLSS・垂直同期・普段のFPS

GPUを750 mVへ下げたあと、FF14のDLSSの適用条件と、NVIDIA Appの表示設定も変えました。

| 設定する場所 | 項目 | 設定値 |
| --- | --- | --- |
| FF14ベンチマーク | 解像度 | 3840×2160 |
| FF14ベンチマーク | スクリーンモード設定 | 仮想フルスクリーンモード |
| FF14ベンチマーク | グラフィック設定のプリセット | カスタム |
| FF14ベンチマーク | グラフィックスアップスケールタイプ | NVIDIA DLSS |
| FF14ベンチマーク | ダイナミックレゾリューション（動的解像度）を有効にする | 有効 |
| FF14ベンチマーク | 適用するフレームレートのしきい値 | 常に適用 |
| FF14ベンチマーク | 3Dグラフィックス解像度スケール | 100 |
| NVIDIA App | DLSSモデルの上書き | Preset K |
| NVIDIA App | 垂直同期 | 高速 |

DLSSの表示品質を調べて、Preset Kにたどり着きました。この設定で見直すと、気になっていたボケやにじみはほぼなくなりました。Preset Kの意味や、RTX 3080で使う理由は[GPU記事の表示品質についての部分](./system-tuning-gpu-750mv-dlss.md#表示品質を調べてpreset-kを試す)に補足しています。

普段のFF14は60 fpsで遊んでいます。キャラクターの細かな動きも楽しみたいので、60 fpsに合わせています。上の設定で回したベンチマークでは平均100 fpsを超えていて、普段遊ぶには十分です。メインで遊んでいるFF11は30 fpsです。
