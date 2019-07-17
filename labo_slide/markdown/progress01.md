# 進捗報告
### 2018 10/25
### 富澤駿

---

## Reference
* McNeill D. "Hand and mind: What the hands reveal about thought." University of Chicago Press, Chicago, IL, USA (1992)
* Kendon, Adam. "Gesticulation and speech: Two aspects of the process of utterance." The relationship of verbal and nonverbal communication 25.1980 (1980): 207-227.
* Mitra, Sushmita, and Tinku Acharya. "Gesture recognition: A survey." IEEE Transactions on Systems, Man, and Cybernetics, Part C (Applications and Reviews) 37.3 (2007): 311-324.

---

## 研究の目標
* 機械対コンピュータのインターフェイス向上に貢献したい
* そのため，ジェスチャの意味を認識できるモデル構築したい
* 先行研究はほどんど無かった
* 似た研究に対しては，以下の新規性があると思われる．
  * 持っているデータが特殊
  * **意味ラベルまでの推定を行う**

---

## 夏季休暇にやってた事
* 関連論文調査
* **ジェスチャに対する知識の補完(認知科学，社会科学)**
* その他手法の復習(HMM,SVM,カーネルトリック,双対問題)

---

実際に何か数値を提示できるまで進んでいないので，

調べた事のまとめを載せて置きます．

---

## 関連研究に関して
* 研究領域キーワード
  * "Gesture recognition"
  * "Gesture Classification"
* これらにもいくつか種類がある
  * Vision based
  * **sequencel based**

---

## ジェスチャに関する理論
* マークニール理論
* **Kendonモデル**

この二人の文献はほとんどの文献で参照されていた．

なので，かなり重要だと思われる．

---

## Kendonモデル

ジェスチャーの構造を定義した．

多くのジェスチャー分析では，このモデルを参考にジェスチャーのモデリングを行う．

---

### 持っているデータのまとめ

---

### 実験設定
* 1セッションは3人で構成
* 合計8?セッションある
* 説明者は動画(Canary Row)を見て、他二人に内容を説明するタスク

---

### 実験に使用した機材
* マイク
* Kinect
* 筋電を取るやつ(Myo Ware???)

---

### 以下の種類のデータがある
* 会話音声
* **Kinectデータ**
* 会話の書き起こし
* 筋電データ

---

## 当面の目標
* ジェスチャフェーズの機能ラベルの推定
* とりあえずKinectデータを入力にしてみる
* 可能なら他クラス問題として定式化した
  * 無理なら特定のラベル検出器(2値分類)

---

## これから決めたい事
* 前処理はどうする?
* 入力データの構造，特徴決定
* 他のデータの活用法
* 使うモデルは？(LSTM?,SVM)
* 具体的な問題設定(他クラス分類？)

