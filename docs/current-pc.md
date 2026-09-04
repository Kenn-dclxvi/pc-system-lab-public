# 現在使っているPC

![現在使っているPCの内部構成](images/current-pc-interior.jpg)

このPCは、Fractal Design Define 7にCore i9-10900KとGeForce RTX 3080を搭載したWindowsのメイン機で、ゲームにも使っています。CPUは360 mmのNZXT Krakenで冷やしています。

最高スコアだけを狙うのではなく、普段使うときの静かさ、消費電力、温度、長時間の安定性のつり合いを見て調整しています。

## 主な構成

| 区分 | 構成 |
| --- | --- |
| ケース | Fractal Design Define 7 |
| マザーボード | ASUS TUF GAMING Z490-PLUS (WI-FI) |
| CPU | Intel Core i9-10900K、10コア / 20スレッド |
| メモリ | G.Skill F4-4000C15D-16GTZR、16 GiB（8 GiB ×2） |
| GPU | ASUS ROG-STRIX-RTX3080-O10G-GAMING、10 GiB GDDR6X |
| CPUクーラー | NZXT KRAKEN Plus 360 RGB v2 |
| ストレージ | Samsung 970 EVO Plus 500GB |
| 電源 | Super Flower LEADEX PLATINUM SE（定格容量と正確な型番は未確認） |

## どういう考えで調整しているか

2026年にAIOの故障をきっかけに、まずケースファンと水冷系を見直しました。この交換では、前面吸気だったラジエーターを天面排気へ移し、前面は140 mmファン3基の直接吸気に変えています。回転数を上げた条件まで測ってみると、強く回しても温度がほとんど変わらないファンがありました。で、現在は必要な冷却を保てる範囲まで回転数を下げ、ラジエーターファンはCPUの瞬間温度ではなく冷却水の温度を見て制御しています。

次に、RTX 3080の電圧・周波数カーブを調整しました。特定のFF14ベンチマーク条件では、約3%のスコアと引き換えに平均消費電力を約100 W下げる結果になりました。ただ、すべての負荷で同じ差になるとは限りません。

その長時間試験でCPU側の不安定性が見つかり、Core i9-10900KのTurbo、TVB、Adaptive Voltage、V/F Curveまで確認することになりました。最初からCPU電圧を調整するつもりだったわけではなく、ひとつずつ測った結果、見る範囲が広がった形です。

詳しい経緯は[2026年 自作PCチューニング記録](../articles/system-tuning-2026-09-01-index.md)にまとめています。現在のCPUとGPUは、コア数に応じた倍率、倍率帯ごとのV/F Point、CPUの90 W上限、GPUの1890 MHz / 850 mVを組み合わせたところまでを、ひとまず基本形としています。

その設定へ至るまでの続きは[CPU効率編](../articles/system-tuning-2026-09-03-cpu-efficiency.md)、現在のPCをFF14ベンチマークの4K最高品質で見直した結果は[4K最高品質のFF14ベンチマーク記事](../articles/system-tuning-2026-09-03-ffxiv-maximum-quality.md)にまとめました。次は、完成した構成が実際の使用や別の負荷でどう動くかを、計測環境も含めて観測していきます。
