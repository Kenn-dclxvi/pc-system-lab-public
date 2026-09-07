# PC System Lab

自作PCの構成と、冷却、GPU、CPUを調整してきた過程を記録しています。設定値だけでなく、何が気になり、どの条件で測り、結果をどう判断したのかも残しています。

![現在使っているPCの内部構成](docs/images/current-pc-interior.jpg)

## いま取り組んでいること

CPUの電圧を詰め、FF14を長時間回したあと、普段使うブラウザでも倍率を変えて性能と消費電力を比べています。7～9コア使用時は47倍に決め、残る1～6コアの倍率を考えているところです。

- 最新の記事：[40倍から探り直して、7～9コアは47倍にする](articles/system-tuning-2026-09-07-browser-ratio-balance.md)
- 今回の調整を最初から：[ファンを静かにしたあと、CPU電圧をもう一度詰める](articles/system-tuning-2026-09-05-ffxiv-voltage.md)

## まず読む

| 知りたいこと | 読むもの |
| --- | --- |
| PCの構成と調整方針 | [現在使っているPC](docs/current-pc.md) |
| 冷却からCPU調整までの流れ | [自作PCチューニング記録](articles/system-tuning-2026-09-01-index.md) |
| FF14で使うCPU倍率と電力 | [最適なコア数を探すところから、FF14で使う電力を選ぶところへ](articles/system-tuning-2026-09-03-cpu-efficiency.md) |
| FF14ベンチマークの4K性能と電力効率 | [FF14ベンチマークを4K最高品質で回し、このPCの実力を測り直す](articles/system-tuning-2026-09-03-ffxiv-maximum-quality.md) |
| 公開している記事の一覧 | [記事一覧](articles/README.md) |

この公開版には、個人情報や端末固有情報を含み得るEvent Log、Raw Data、ソフトウェア一覧、運用設定、完全な会話ログを含めていません。記事中の数値は、非公開の記録用リポジトリに保存した測定結果をもとにしています。

## ライセンス

このリポジトリで公開する独自の文章と写真は、特記がない限り[Creative Commons Attribution 4.0 International](LICENSE.md)（CC BY 4.0）で利用できます。再利用するときは`Kenn-dclxvi`を作者として示し、このリポジトリとライセンスへのリンクを残してください。変更した場合は、そのことも示してください。

FFXIVベンチマーク画面など、第三者に権利がある画像はCC BY 4.0の対象外です。該当画像は掲載ページの権利表記と[ファイナルファンタジーXIV 著作物利用条件](https://support.jp.square-enix.com/rule.php?id=5381&tag=authc)に従って扱ってください。

今後は、公開できる試験結果を記事として追加し、PC全体の写真も撮れたら内部写真と分けて載せたいと思います。
