# kino.UiPath.MailAttachments

メールを受信するアクティビティから取得できる ``System.Net.Mail.MailMessage`` から添付ファイルを取り出すアクティビティ(Mail2Attachments)です。

取り出したデータはBase64エンコードされた文字列なので、たとえばそのままOCのキューに投入したりすることが可能です。

また、そのBase64エンコードされた文字列をバイト列(Byte[])へ戻すアクティビティ(Base64Decode)や、そのバイト列をファイルに書き出すアクティビティ(Bytes2File)などもあります。


## 画面イメージ

### Mail2Attachments

引数の ``System.Net.Mail.MailMessage`` から、添付ファイルを取り出し、Base64エンコードした文字列や、ファイル名を取得します。メールには複数ファイル添付されている可能性があるため、それぞれが配列になっています。

![01.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/73777/311813b7-f343-926c-ef18-e7ad66ee1486.png)

- Input
    - message: IMAPメールメッセージを取得アクティビティから得られる System.Net.Mail.MailMessage のオブジェクト
- Output
    - count: 取得した添付ファイルの件数
    - fileNames 取得した添付ファイルのファイル名(複数なので配列)
    - base64s: 取得した添付ファイルのBase64エンコードされた文字列(複数なので配列)


### Base64Decode

``target``に渡された、Base64エンコードされた文字列をDecodeして、Outputの変数に Byte[]として格納します。

![02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/73777/1cc4d836-3ce9-7683-9047-7f30dd39e225.png)


- Input
    - target: Base64エンコードされた文字列
- Output
    - bytes: DecodeされたバイナリデータをByte[]として格納


### Bytes2File

バイナリデータByte[]を、指定したフルパスへ書き出します。

![03.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/73777/3acfeb84-f81a-c94f-a8a3-ca1324d80adf.png)

- Input
    - bytes: 出力したいバイナリデータ(Byte[])
    - fullPath: 書き出したいパス。もともと添付ファイルには Mail2Attachments から``fileNames``というプロパティでファイル名取れるので、「所定のディレクトリ+fileNames(index番号)」などと指定して、添付ファイル群を書き出すことが可能です。


## ソースコード

https://github.com/masatomix/kino.UiPath.MailAttachments
    

## 改訂履歴

- 0.1.0 Base64Decodeしたり、Byte列をファイル出力したり、それらをテストするコードを追加。
- 0.0.9 初版