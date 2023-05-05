# 深層学習画像分類：VGG
## ポイント
- Convのkernel_sizeを小さく(3×3)することによって、深層ネットワーク構造を深く構築。これにより分類精度が向上
- AlexNetで使用されていたLocal Response Normalization（LRN）正規化を含んでいない（Krizhevsky他 2012）。なぜなら、精度向上が見込めず、メモリ消費および計算時間の増加を招くから。
- 画像を[Smin, Smax]でリサイズ。そして、224サイズでクロップし、マルチスケールで学習を行い、汎化性能を向上させた。
## kernel_sizeを小さくした効果
- ネットワークを深く構築できる
- 使用パラメータの削減:
フィルターサイズ3×3の畳込み層3つは、フィルターサイズ7×7の畳み込みの効果がある。 
3層（3×3）畳み込みスタックの入力と出力の両方にCチャネルがあると仮定すると、スタックは3×（3×3×C×C）=27×C×Cの重みでパラメータ化される。単一7×7畳込み層のフィルターサイズは7×7×C×C＝49×C×Cの重みパラメータ。
**すなわちパラメータサイズを81%以上削減し、同時に正則化の効果が得られる。**
## アーキテクチャ図
<img alt="VGG architecture" src="./image/vgg-archi.avif">
## トレーニング方法
- ミニバッチ勾配降下法で最適化
- モーメンタム:0.9
- 学習率は当初0.01に設定。その後検証セットの精度が向上しなくなったときに1/10倍。
- 重み減衰（L2ペナルティ乗数を「5×10の-4乗」に設定）
- バッチサイズは256
- 最初の2つの全結合層のドロップアウト正則化（ドロップアウト率を0.5に設定）
- データオーギュメンテーション: リスケーリングされたトレーニング画像からランダムにクロップ(224×224)。その後、ランダムな水平反転とランダムなRGBカラーシフトを適用。
- ネットワークの重み初期値を設定するために、浅い構成Aから訓練する。その後、最初の4つの畳み込み層と最後の3つの全結合層を構成Aの層で初期化（中間層はランダムに初期化）。
**※しかし、上記手段を使わずとも、Glorot＆Bengio（2010）のランダム初期化手順を使用することによって事前トレーニングなしに重みを初期化することが可能。**
## 参考
1. https://qiita.com/ttomomasa/items/b673a1e0b42a2a14a9d2
