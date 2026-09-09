# 現在使っているPC

![現在使っているPCの内部構成](images/current-pc-interior.jpg)

このPCは、Fractal Design Define 7にCore i9-10900KとGeForce RTX 3080を搭載したWindowsのメイン機で、ゲームにも使っています。CPUは360 mmのNZXT Krakenで冷やしています。

普段の操作やゲームを快適に楽しみながら、消費電力とファンの音を抑えて使う方針です。

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
| 電源 | Super Flower LEADEX PLATINUM SE 1000W |

## 調整方針

### 冷却と静音性

前面3基のファンを吸気、背面ファンと天面ラジエーターを排気に使い、底面ファンは停止しています。ケースファンはCPUやGPUの温度、水冷系は冷却水の温度を見て回します。

負荷に応じた冷却を保ちつつ、待機中は回転数を抑えて静かに使えるようにしています。

### CPUの性能と消費電力

CPUは、少数コアの処理を速く動かせるように高倍率を残しています。多くのコアを使うときは倍率を下げ、普段のブラウザ操作やゲームに必要な性能と消費電力の両方を見て、倍率や電圧を決めています。

### GPUとゲームの表示品質

GPUは、普段のゲームが滑らかに動き、画面の見え方にも納得できる範囲で電圧とクロックを下げています。FF14ではDLSSを常時適用し、NVIDIA AppでPreset Kを指定しています。

キャラクターの細かな動きも楽しみたいので、FF14は60 fpsで遊んでいます。メインで遊んでいるFF11は30 fpsです。

### 長く使うための動作確認

設定を決めるときは、普段のブラウザ操作や実際のゲームに加え、長時間の負荷でも動作を確認しています。温度や消費電力とともに、途中でエラーが出ないか、動きや表示に気になるところがないかも見ています。

## 設定値と調整の記録

倍率・電圧・ファンカーブなどの具体的な値は[このPCのBIOS・アプリ設定](../articles/system-tuning-settings.md)にまとめています。比較した結果や設定を選ぶまでの経緯は、[自作PCチューニング記録](../articles/system-tuning-2026-09-01-index.md)で読めます。
