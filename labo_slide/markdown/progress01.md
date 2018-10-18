# 進捗報告
### 2018 10/25
### 富澤駿

---

## Reference
* McNeill D. "Hand and mind: What the hands reveal about thought." University of Chicago Press, Chicago, IL, USA (1992)
* Kendon, Adam. "Gesticulation and speech: Two aspects of the process of utterance." The relationship of verbal and nonverbal communication 25.1980 (1980): 207-227.
* Mitra, Sushmita, and Tinku Acharya. "Gesture recognition: A survey." IEEE Transactions on Systems, Man, and Cybernetics, Part C (Applications and Reviews) 37.3 (2007): 311-324.

---

## 研究内容
* 機械対コンピュータのインターフェイス向上のため，ジェスチャの意味を認識できるモデル構築したい
* 先行研究はほどんど無かった
* 似た研究に対しては，以下の新規性があると思われる．
  * 持っているデータが特殊
  * **意味ラベルまでの推定を行う**

---

## 夏季休暇にやってた事
* 関連論文調査
* ジェスチャに対する知識の補完(認知科学，社会科学)
* その他手法の復習(HMM,SVM,カーネルトリック,双対問題)

---

## 当面の目標
* ジェスチャフェーズの機能ラベルの推定
* とりあえずKinectデータを入力にしてみる

---

## 問題点
* 前処理は？
* 筋電とかのデータもある．そのうちそれも使いたい．
* 使うモデルは？(LSTMとかはデータが足りなそう)
* 具体的な問題設定(他クラス分類？)

---

実際に何か数値を提示できるまで進んでいないので，

調べた事のまとめを載せて置きます．

---

## 関連研究に関して
* 研究領域キーワード
  * "Gesture recognition"
  * "Gesture Classification"
* これらにもいくつか種類がある
  * Vision based
  * **sequencel based**

---

## 目次
1. ジェスチャに関する重要な理論
1. a
1. a
1. a
1. a

---

### ジェスチャに関する理論
* マークニール理論
* Kendonモデル

---

この二人の文献はほとんどの文献で参照されていた．

かなり重要だと思ったので，簡単にそれぞれ述べておく．

---

# マークニール理論

---

# Kendonモデル

---

### Gesture Classification の種類
* 先行研究に関してはそんなに多くない
* いくつかの種類が存在している
  * Vision ベースのタスク
  * **信号ベースのタスク**

---

### 持っているデータのまとめ

---

### 実験設定
* 1セッションは3人で構成
* 合計8?セッションある
* 説明者は動画(Canary Row)を見て、他二人に内容を説明するタスク

---

### 実験に使用した機材
* 指向性マイク
* モーションキャプチャ
* 筋電を取るやつ(Myo Ware???)

---

### 以下の種類のデータがある
* 会話音声
* 姿勢データ
* センサーデータ
* 筋電データ

---

### データ詳細1
* Audio
  * Day~~~segment(指向性マイクの元データ????)
  * Sengment(キネクト音声の元データ????)
  * AudioTimeStamp.csv(キネクトの音声を処理後)
    * Gestureがあった場合label=1、そうでない場合=0
* Body
  * BodyData.csv(モーションキャプチャのデータ)

---

### データ詳細2
* day~~.wav(指向性マイクの音声？)
* Day~~...ment.csv(韻律区間+書き起こしデータ)
* Depth(これの正体が不明です????)
* Gesture Anotation(ELAN用のフォルダ)
* Myo(筋電。"~１.txt"、"~２.txt"の違いは左右????)
  * acc(????)
  * emg(筋電????)
  * gyro(ジャイロ????)
  * ori(????)

---
