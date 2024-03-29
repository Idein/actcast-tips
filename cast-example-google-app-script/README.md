# Google App ScriptとのCast連携

## 概要


ActcastのCast機能を使用することで、GAS（***Google App Script***）を介して直接的にGoogle Spreadsheetにデータを書き込むことが可能です。この方法は、IFTTTを介するよりも制限が少なく、セキュリティリスクが低いという利点があります。

＜IFTTTと比べたメリット＞

- アプレット数の制限がない： IFTTTの無料版では最大2つのアプレットしか作成できませんが、ActcastのCast機能を使用する場合は制限がありません。必要に応じて、複数のアクションを作成してGoogle Spreadsheetにデータを書き込むことが可能です。
- 経由の削減： ActcastのCast機能を使用することで、データの送信経路が簡略化されます。IFTTTを介する場合は、ActcastからIFTTTへの送信、そしてIFTTTからGoogle Spreadsheetへの書き込みという二段階のプロセスが必要ですが、Actcastを直接使用することで、経由するサービスが減ります。
- セキュリティリスク低減：ActcastのCast機能を使用する場合、IFTTTを介する必要がないため、IFTTTにアカウント情報や送信データ内容を渡す必要がありません。これにより、データのセキュリティリスクが低減します。

## 設定方法

事前に、Google アカウントの登録が必要です。登録済みのアカウントがある場合は、新規に登録する必要はありません。

### **Google SpreadsheetとGASの設定**

**1.空のスプレッドシートを作成します。**

- 任意のファイル名をつけます。ファイル名は設定後に変更してもアドレスが変わる事はありません。
- URLをコピーします。
- 「拡張機能」から、「Appscript」を開きます。
    
    ![untitled.png](https://github.com/Idein/actcast-tips/assets/106148688/6b1baff6-88c4-4fc9-ab8b-84f1d05d0ac1)
    

**2. GASを編集します。**

下記をGAS編集画面に貼り付けます。

**3行目のURLを1にてコピーしたURLに変更します。** Cast内容に応じてパラメータ名も変更します。

```
function doPost(e) {
  // スプレッドシートを開く　URLは先程作成したスプシのアドレスに変更する。
  const url = "https://docs.google.com/spreadsheets/d/1t8se-HM＊＊＊＊UdOi-cm1Wg0Mc3g8PonKsevjNCTn4o52E7C＊＊＊＊＊Q/edit#gid=0";
  const ss = SpreadsheetApp.openByUrl(url);
  const sheet = ss.getSheets()[0];

  // 受け取ったJSONデータをパース
  const jsonData = JSON.parse(e.postData.contents);

  // JSONデータからパラメータを取得　適宜パラメータ名を変更する。
  const params = {
    "area_name": jsonData.area_name,
    "on": jsonData.on,
    "off": jsonData.off
  };

  // スプレッドシートにデータを追加
  sheet.appendRow(Object.values(params));

  // 成功メッセージを返す
  return ContentService.createTextOutput('success');
}
```

**3. デプロイ**
    
   GAS記述内容をデプロイします。
    
   「デプロイ」を選択し、種類を「ウェブアプリ」に設定します。　
    
   次のユーザーとして実行は、自分のアカウントを設定します。
    
   アクセスできるユーザーは「全員」。許可設定などがでてきたら許可します。
    
   ![untitled.png](https://github.com/Idein/actcast-tips/assets/106148688/06bd2355-d342-4121-81eb-b857983075a5)
    
   「デプロイ」を押すと、URLが生成されるのでコピーします。
    
   ![untitled.png](https://github.com/Idein/actcast-tips/assets/106148688/bb21e965-0b8a-4f97-94c6-8ba533d57178)
    
   スプシ側の設定は以上です。
    
  ### Cast設定

actcastにログイン後、左メニューから`任意のアプリケーション > Casts`を選択し、`Make New Cast`ボタンをクリックしてください。

![make_new_cast.png](https://github.com/Idein/actcast-tips/assets/106148688/70a480d3-0ee6-416c-a1d3-5da14eca9d3d)

まずはトリガーを設定します。 ここでは無条件ですべてのデータを送信させるために、何も設定せずに`Configure Action`をクリックします。

続いてアクションを設定します。

![cast_config.png](https://github.com/Idein/actcast-tips/assets/106148688/a71606c9-86b5-4413-897f-396dba884ec0)
    
### CAST設定画面にて、URLにデプロイにて生成されたURLを貼り付ける。
   
 その他設定は下記の通り。「Body」のパラメータ名はGAS側と合わせる必要がある。
    
  `Webhook`を選択し、 Webhook 用の設定画面を表示します
    
   `URL`には先ほど作成した関数のエンドポイントを入力します
    
   `Method`や`Headers`、`Body`に関しては、関数内で実装した内容に応じて設定します
    
   `Finish`をクリックすると Cast が作成されます。
    
   <img width="624" alt="スクリーンショット 2023-11-06 13 08 46" src="https://github.com/Idein/actcast-tips/assets/106148688/1e99a0c7-9f85-4084-9bef-5900c6e9227d">
    
以上で連携作業は完了となります。
    
   Castについて詳しい設定や仕様が知りたい場合には、Actcast Documents内の[Cast](https://actcast.io/docs/ja/ActManagement/Cast/) の項目をご確認ください。

 ## rate limitについて    
アクションが Webhook である Cast はレート制限の設定を持ちます。 レート制限の設定値とは Cast の URL が指す Cast 対象サービスに対して一定期間内に送出できるリクエストの回数の上限を表します。 Act に登録されているデバイスたちがこの上限以上の Act Log を送出しようとした場合、Actcast は上限を超える分を Cast 対象サービスにリクエストすることなく破棄します。
レート制限やその解除方法についてはActcast Documents内の[Castのレート制限](https://actcast.io/docs/ja/ActManagement/Cast/WebhookCastRateLimit/)に関する項目をご参照ください

   ## 免責
    
   当社は、当社以外の第三者により管理・運営されるサービス（以下「第三者サービス」といいます。「Google Spreadsheet」および、「Google App Script」はこれに該当します。）と当社サービスをCastを用いて連携させる機能を提供していますが、その連携や連携の継続を保証するものではありません。
    
   当社は、当社サービスと連携またはリンクされている第三者サービスの正確性、信頼性、完全性、有用性およびセキュリティにつき、いかなる保証もいたしません。第三者サービスの詳細や仕様、最新の情報については、第三者サービスの管理・運営者の公式サイト等を参照し、最新の情報を確認いただくことを強くお勧めします。
    
   お客様は、第三者サービスの利用をお客様の責任において行うものとし、当社は第三者サービスの利用によりお客様又は第三者に生じた一切の損害について何らの責任を負いません。また、お客様は、第三者サービスの利用について当該第三者サービスの利用規約、契約等に従うものとし、自らの責任でこれらの内容を確認し、遵守してください。
    
   Castを用いた当社サービスと第三者サービスとの連携に関して問題や疑問が生じたときは、当社までお問合せいただければ、可能な範囲でサポートさせていただきます。しかし、第三者サービスの使用方法やサービスに関するお問合せについては、第三者サービスの公式サポート窓口等をご利用いただきますようお願いいたします。