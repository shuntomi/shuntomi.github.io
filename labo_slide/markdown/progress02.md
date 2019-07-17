### マルチモーダル情報を利用した，
### 会話中のジェスチャー機能の推定．
###    
#### 2019 7/18 進捗報告
#### 富澤駿

---

1. <font color="Red">Abstract</font>
1. 研究背景
1. 関連研究
1. 研究目的
1. 提案手法 
1. 前回までの進捗
1. 今回までの進捗
1. 今後の予定

---

## Abstract
* 機械対コンピュータのインターフェイス向上に貢献したい
* そのためには言語情報に加えて非言語情報の認識が重要である．
* ジェスチャは場面によって何かしらの機能を持っていると考えられる．
* 本研究の目的は，ストーリーテリングタスクにおいて発生するジェスチャーの機能を識別するモデルを提案することである．

---

1. Abstract
1. <font color="Red">研究背景</font>
1. 関連研究
1. 研究目的
1. 提案手法 
1. 前回までの進捗
1. 今回までの進捗

---

## 研究背景 1
* コミュニケーションにおいて，言語情報に加えて非言語情報(ジェスチャー，視線，韻律)も重要な役割を担っている．
* 特にジェスチャーは会話内容，話者の意図を理解する上で重要な非元言語情報だと考えられる．
* ジェスチャーに関する研究は社会心理学，言語学などで多く行われており，これらの知見を利用する事ができる．
* 特にジェスチャーの構造に関しては，Kendonらが提案した理論によって構造化が可能である．

---

## 研究背景 2
* 近年，画像や時系列データを使用し，コンピュータにジェスチャーを自動認識させる研究が盛んに行われている．
* 意図的に発生させたジェスチャーの分類を目的とした研究や，発話と共に発生するジェスチャーの研究，分類を目的とした研究が存在する．
* またジェスチャーの検出に加えて，ジェスチャーフェーズの自動分割を目指す研究も行われている．

---

1. Abstract
1. 研究背景
1. <font color="Red">関連研究</font>
1. 研究目的
1. 提案手法 
1. 前回までの進捗
1. 今回までの進捗

---

## 関連研究 1
* SSPサーベイ[Vinciarelli et al.,2009]
* ジェスチャーとコミュニケーション[Mcneel 1992]
* ジェスチャーの構造に関して[Kendon 2004]
* ジェスチャーの機能[]

---

## 関連研究 2
* ジェスチャー認識サーベイ[Mitra et al.,2004]
* SVMによるジェスチャーフェーズの自動セグメンテーション(kinect)[Mitra et al.,2004]
* CNNLSTMによるジェスチャー認識(画像シーケンス)[Tsironi et al.,2017]
* ロジスティック回帰によるジェスチャースローク(手の位置，角度，座標)[Gebre et al.,2017]
* グループ対話におけるジェスチャ機能認識(韻律，頭，手)[Okada,S et a.]

---

### Reference 1
* Vinciarelli, Alessandro, Maja Pantic, and Hervé Bourlard. "Social signal processing: Survey of an emerging domain." Image and vision computing 27.12 (2009): 1743-1759.
* D. McNeill, Hand and Mind: What Gestures Reveal about Thought, University of Chicago Press, 1992.
* A. Kendon, Gesture: Visible Action as Utterance,Cambridge University Press, 2004.

---

### Reference 2

* Mitra, Sushmita, and Tinku Acharya. "Gesture recognition: A survey." IEEE Transactions on Systems, Man, and Cybernetics, Part C (Applications and Reviews) 37.3 (2007): 311-324.
* Madeo, Renata Cristina Barros, Sarajane Marques Peres, and Clodoaldo Aparecido de Moraes Lima. "Gesture phase segmentation using support vector machines." Expert Systems with Applications 56 (2016): 100-115.
* Tsironi, Eleni, et al. "An analysis of convolutional long short-term memory recurrent neural networks for gesture recognition." Neurocomputing 268 (2017): 76-86.
* Gebre, Binyam Gebrekidan, Peter Wittenburg, and Przemyslaw Lenkiewicz. "Towards automatic gesture stroke detection." LREC 2012: 8th International Conference on Language Resources and Evaluation. European Language Resources Association, 2012.
* Okada, Shogo, et al. "Context-based conversational hand gesture classification in narrative interaction." Proceedings of the 15th ACM on International conference on multimodal interaction. ACM, 2013.

---

1. Abstract
1. 研究背景
1. 関連研究
1. <font color="Red">研究目的</font>
1. 提案手法 
1. 前回までの進捗
1. 今回までの進捗

---

## 研究目的

マルチモーダル情報を利用し，ジェスチャーの機能を推定するモデルを構築する事．

先行研究の目的の多くはジェスチャーの機能や構造の推定であるが，

それらが持つ機能の推定を目指す点が先行研究と異なる

---

1. Abstract
1. 研究背景
1. 関連研究
1. 研究目的
1. <font color="Red">提案手法</font>
1. 前回までの進捗
1. 今回までの進捗

---

## データセットについて
* 実験は説明者と聞き手が存在する．
* 説明者は事前に２本のショートムービー(1本約10分)を見て，内容を聞き手に説明する．
* 36人×2=72セッションのデータを取得した．
* このうち不備のあったデータを除く67セッション分を使用

---

## データ詳細
* kinect,マイク，Myoを使用してデータを取得
* kinectによって,30fpsでサンプルし以下の13部位の姿勢データを取得した．
    * 頭(1)
    * 両肩,両肘(2×2=4)
    * 左右手首，手，手先，親指(4×2=8)

---

## アノテーションについて
* 観測されたジェスチャーは専門家によって「レスト，ストローク，ホールド」のジェスチャーフェーズにセグメントされている．
* この内ストローク部分に対して，機能ラベルが付与されている(重複する場合あり)．
* ストロークの数は4772個

---

### 機能ラベルについて
* 説明(2027)
* 発話調整(1128)
* ワードサーチ(328)
* 自己接触(303)
* 言語強調(137)
* 言い淀み(11)
* ビート(703)
* その他(85)

※ビートは他のラベルと重複している物がある．
    
---

### 問題設定
多クラス分類問題として定式化する．

単純化のためジェスチャーフェーズの分割は既知とする．

* 入力: ストローク部分に該当する特量量
* 出力: 機能ラベルに該当するクラス

---

### 本研究の流れ
1. 特徴量抽出(どの部位がいるのか？時系列情報はいるのか？)
1. 特徴量選択
    1. T検定,LDA?
1. 機械学習
    1. SVM, XGboost など
1. モデルの評価

---

1. Abstract
1. 研究背景
1. 関連研究
1. 研究目的
1. 提案手法 
1. <font color="Red">前回までの進捗</font>
1. 今回までの進捗

---

## 前回までの進捗
* 関連論文調査
* データ整備
* ジェスチャに対する知識の補完(認知科学，社会科学)
* その他手法の復習(HMM,SVM,カーネルトリック,双対問題)

[前回資料](../progress01.html)

---

1. Abstract
1. 研究背景
1. 関連研究
1. 研究目的
1. 提案手法 
1. 前回までの進捗
1. <font color="Red>今回までの進捗</font>

---

### 当面の目標
とりあえず，姿勢データのみを使って一定の精度を出す．

機械学習のアルゴリズムより特徴量抽出手法がキーになると思われる．

それをベースラインとして，視線，表情，音声などを特徴量に加え精度を向上させたい．

---

### 現状の進捗

1. 特量量抽出方法をいくつか試した．(kinect姿勢データ，速度と加速度，肩重心との位置ベクトルのノルム，時間長，左右の手の距離など)
1. T検定を利用し，特量量の選択を行った．
1. 正例，不例の数に偏りがあるラベルに関してはSMOTEによるオーバーサンプリングを行った．
1. 評価

---

#### 特量量抽出詳細
前処理として，各セッションごとに正規化し，

各部位と肩の重心との差分をとった．

時系列情報は潰し，以下の平均と分散を特徴量として使用した．
1. 13部位の位置，速度，加速度，時間長，一つ前がホールドかどうか(13×3×3 + 2 = )
2. 13部位の位置，速度，加速度のノルム，両手の距離，時間長，一つ前がホールドかどうか

---

### 実験

---

### 考察
* SVMうんぬん以前に特徴量が上手くとれていない．
* 結果を見ると，あまり複雑な特量量じゃないほうが良さそう．

---

## 日程経過と進捗実態

* 7月: 姿勢データに絞って特徴量抽出と特量量選択を試す．
* 8月: 他クラス分類問題への拡張.中間発表に準備
* 9月: 他のモダリティの導入(音声は入れずに顔の特量量を加えてみたい) 
* 10月: 他の機械学習手法を試す(特にニューラールネット系)

---

[論文紹介へ続く](../paper03.html)