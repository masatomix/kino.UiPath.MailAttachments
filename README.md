# kino.UiPath.MailAttachments

メールを受信するアクティビティから取得できる ``System.Net.Mail.MailMessage`` から、添付ファイルを取り出すアクティビティ(Mail2Attachments)です。


![01.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/73777/311813b7-f343-926c-ef18-e7ad66ee1486.png)

取り出した添付ファイルデータは、Base64でEncodeされた文字列にしてあるので、たとえばそのままUiPath Orchestrator(以下OC)のキューに投入したりすることが可能です。

UiPath標準のアクティビティ「添付ファイルを保存」だと、添付ファイルをファイルに保存する機能しかなかったので(ですよね？？)、ファイルを書き出すことなくOCのキューに投入したいとおもい、作成しました。

また、Base64Encodeされた文字列をバイト列(Byte[])へ戻すアクティビティ(Base64Decode)や、そのバイト列をファイルに書き出すアクティビティ(Bytes2File)なども作成しました。なので取り出したBase64Encode文字列をBase64Decodeアクティビティにかけて、さらにBytes2Fileアクティビティに渡してあげれば、標準の「添付ファイルを保存」アクティビティと同じ機能になります。

## インストール

https://www.nuget.org/packages/kino.UiPath.MailAttachments/ で登録してあるので、UiPath Studioの「パッケージを管理 >> すべてのパッケージ」を選択し``kino.uipath``で検索してみると一覧されると思います。そのままインストールしてください。

UiPath Studio 2019.10.12 で動作確認しています。

## 画面イメージ

### Mail2Attachments

引数の ``System.Net.Mail.MailMessage`` オブジェクトから添付ファイルを取り出し、Base64Encodeした文字列や、ファイル名を取得します。メールには複数ファイルが添付されている可能性があるため、データ文字列、ファイル名それぞれが配列になっています。

![01.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/73777/311813b7-f343-926c-ef18-e7ad66ee1486.png)

- Input
    - message: 「IMAPメールメッセージを取得」アクティビティから得られる System.Net.Mail.MailMessage のオブジェクト
- Output
    - count: 取り出した添付ファイルの件数
    - fileNames 取り出した添付ファイルのファイル名(複数なので配列)
    - base64s: 取り出した添付ファイルの、Base64Encodeされた文字列(複数なので配列)


### Base64Decode

``target``に渡された、Base64Encodeされた文字列をDecodeして、Outputの変数に Byte[]として格納します。

![02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/73777/1cc4d836-3ce9-7683-9047-7f30dd39e225.png)


- Input
    - target: Base64Encodeされた文字列
- Output
    - bytes: DecodeされたバイナリデータをByte[]として格納


### Bytes2File

バイト列Byte[]を、指定したパスへファイル保存します。

![03.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/73777/3acfeb84-f81a-c94f-a8a3-ca1324d80adf.png)

- Input
    - bytes: ファイル保存したいバイナリデータ(Byte[])
    - fullPath: 保存先のパス。


## ソースコード

https://github.com/masatomix/kino.UiPath.MailAttachments
    

## 改訂履歴

- 0.1.1 注釈をつけるなど。リファクタリング。
- 0.1.0 Base64Decodeしたり、Byte列をファイル出力したり、それらをテストするコードを追加。
- 0.0.9 初版
