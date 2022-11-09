# 建物用途推定モデル

建物名称からその建物用途を推定します

本モデルの学習には深層学習の一つであり畳み込みニューラルネットワークを自然言語処理に拡張したTextCNNを用いています



# 推定可能な種別

|名称|番号|説明|
|:---:|:---:|:---|
|商業|0|一般消費者向けの商業施設|
|事業所|1|企業やオフィスや営業所|
|公共|2|学校や役所などの公共施設|
|宗教|3|寺社仏閣とその関連施設|
|駐車場|4|駐車場とそれに類する建物|


# 初期設定

1. `git clone https://github.com/ActScapeLab-2022/building-type-estimation-model.git`によってこのレポジトリをダウンロードする

2. 必要なデータをダウンロードする
    - 作者の環境は「Windows 11」の「Python 3.9.1」となっている
    - 展開した場所をカレントディレクトリとして`pip install -r requirements.txt`を実行する

3. 学習済みモデルをの導入
    1. 展開した場所に`Source`フォルダを作成する
    1. テキストデータのベクトル化で使用する学習済みモデルを[ダウンロード](http://www.cl.ecei.tohoku.ac.jp/~m-suzuki/jawiki_vector/data/20170201.tar.bz2)する
    1. ダウンロードしたファイルを解凍し，`entity_vector.model.bin`を`Source`フォルダ内に保存する

4. コマンドプロンプトを展開した場所で起動し`py predict.py セブンイレブン`と入力し、以下の応答を得られれば設定は終了
    ```
    ###  R E S U L T  ###
    
    セブンイレブン：商業
    ```



# 利用方法

2通りの使い方を想定して実装されています

基本的にはコマンドプロンプトから実行することになります

カレントディレクトリを展開した場所にしておいてください

注）本モデルは日本語の建物名称を推定することを目的としており、外国の建物名称には対応していませんのでご了承ください


- 単発の名称について推定したい場合
  - `py predict.py 推定したい名称`と入力することで結果を得ることができます
- ファイルに格納されている大量の名称を一括して推定したい場合
  - `.dbf, .shp, .shx`と`.csv`の2種類をサポートしています
  - 推定したい名称はすべて「name」と名付けられた列に格納してください（`predict_config.json`にて変更可能）
  - predict / input に推定するファイルを格納してください
  - `py predict.py`とすることで推定処理が実行されます
  - 計算が終わると predict フォルダ内にファイルが出力され、予測結果は`predict_config.json`の「predictedNameColumn」にて設定した列に保管されます


# 細かい設定

予測処理に関する設定は`predict_config.json`を編集することで設定できます

|項目名|取りうる値|説明|
|:---:|:---:|:---|
|buildingNameColumn|文字列|入力データにおいて推定する名称が入った列名|
|predictedNameColumn|文字列|出力データにおいて推定結果を格納する列名|
|encoding|文字列|読み込むファイルのエンコーディング|
|isNormalize|true / false|固有名詞を排除した名称の推定を行う|
|rePredicate|true / false|結果出力済みのファイルに対して再度推定を行う|
|numberOutput|true / false|予測結果を種別の名称ではなく，確率で出力する|
|modelName|文字列|models 内のフォルダ名を指定することで，そのフォルダに格納されているモデルを用いて種別の推定を行う|


# 諸注意

- 本モデルは平均正答率80%程度のモデルになります
- 学習件数の少ない「宗教」「駐車場」についてはほかの種別に比べて予測精度が低いです
- バグなどについては、Issuesへ投稿してください



# ライセンス

当プロジェクトは「Apache License 2.0」を適用しています。

商用利用を含めた使用や改変、再配布等を許可していますが、その際には注意点がいくつかありますので、詳細はLICENSEの文面をご確認ください。

また、改変や再配布等で発生したいかなる問題に対して作者は責任を負いかねますので、ご了承ください。


# 論文

- [藤條 嵩大, 大山 雄己, 杉山 航太郎 (2022) : ミクロ土地利用分析に向けた建物用途推定モデルの開発. 人工知能学会全国大会論文集 Vol.36.](https://www.jstage.jst.go.jp/article/pjsai/JSAI2022/0/JSAI2022_3N4GS1002/_article/-char/ja/)

- [藤條 嵩大, 大山 雄己 (2022) : 建物名称に基づく建物用途推定のための深層学習モデルの開発と検証 -ミクロ土地利用分析への適用-. 都市計画論文集 Vol.57 No.3 1025-1032.](https://www.jstage.jst.go.jp/article/journalcpij/57/3/57_1025/_article/-char/ja/)
