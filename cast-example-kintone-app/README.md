# Kintone APIとのCast連携

## 概要

Kintoneはサイボウズ株式会社が提供するサービスであり、特定のイベントや条件に基づいて、Kintone内のアプリへデータを送ることができます。

Kintoneに関する最新の情報や詳細については、以下の公式URLをご参照ください。

[Kintone公式サイト](https://kintone.cybozu.co.jp/)

Kintoneと連携することにより、任意の内容をCast機能を通じて送信することができます

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/82d90074-157c-4fc8-ab76-0a9b269ed203)

## 制約(2024/04/01現在)

Kintoneヘルプのページ内に制限値一覧の項目がございますので、そちらをご確認ください

[制限値一覧|kintone ヘルプ](https://jp.cybozu.help/k/ja/admin/limitation/limit.html)

## 連携方法

設定されているKintoneのログインページよりログインを行ってください

下図赤枠のアイコンをクリックして、Kintoneのアプリを作成を開始します

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/afd57130-7a80-4953-8135-adeea00ace12)

アプリの名前を入力し、必要なフィールドをドラッグアンドドロップで追加していきます

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/f45a339d-f877-4511-ad33-85925b18b9a9)

フィールドの歯車アイコンをクリックし、各フィールドの詳細設定を行っていきます

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/7a269682-2e6b-4154-b03e-204790c173b8)

Kintone上のGUIに従ってフィールド情報を入力します。

大体の場合、必須情報は以下２つです

**フィールド名：**

Kintone上で表示されるフィールド名（カラム名）です。

**フィールドコード：**

APIなどからフィールドを指定する場合このコードを使用します。

**例）**

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/76785e44-fdb2-40be-8aab-e1aefcbd8fcc)

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/d6d83f22-646d-4784-a487-b084869cd431)

すべてのフィールドの設定が終わったら、アプリを公開します

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/8cc65e70-ccb6-4ec5-b6c6-443707ce52cd)

アプリの設定を開き、「設定」タブから「APIトークン」の設定画面を開きます

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/32ff027e-c1c9-4fb1-b057-a54e9e1a3af5)

「生成する」ボタンをクリックしてAPIトークンを発行します。
アクセス権の設定チェックボックスで許可したい操作をチェックしてください。

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/060c6b97-eb71-4ef4-994a-7a2ba81f3138)

Actcastアプリとの連携設定を行う場合は、kintoneのサブドメイン名とアプリID
前項で作成したトークンを控え、actcastのCast設定からご利用ください

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/33f6353b-b7e6-4963-b4f3-499aa94e4412)

## Cast設定

actcastにログイン後、左メニューから`任意のアプリケーション > Casts`を選択し、`Make New Cast`ボタンをクリックしてください。

![make_new_cast.png](https://github.com/Idein/actcast-tips/assets/106148688/70a480d3-0ee6-416c-a1d3-5da14eca9d3d)


まずはトリガーを設定します。 ここでは無条件ですべてのデータを送信させるために、何も設定せずに`Configure Action`をクリックします。

続いてアクションを設定します。

![cast_config.png](https://github.com/Idein/actcast-tips/assets/106148688/a71606c9-86b5-4413-897f-396dba884ec0)


`Webhook`を選択し、 Webhook 用の設定画面を表示します

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/0d5ca6b8-1c52-4a62-82f0-9cd1e77344fe)

以下のように設定を行ってください。

```jsx
URL:https://【YourDomain】.cybozu.com/k/v1/record.json
Method:POST
Headers:Content-Type:application/json
X-Cybozu-API-Token:【YourToken】
Body:{ "app": 【YourAppID】,
 "record": {
"timestamp"   : {"value": "{{ data.timestamp }}"   },  
"line_id"     : {"value": "{{ data.line_id }}"        },       
"forward"     : {"value": "{{ data.forward }}"      },       
"backward"    : {"value": "{{ data.backward }}" }
}
}
```

取得したサブドメイン名を【YourDomain】に、APIトークンを【YourToken】に、アプリIDを【YourAppID】に設定してください。

入力が完了すると、以下の図のような状態になります。

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/e61cfcc2-b0d3-4508-9ed5-b0e4d51cf8cb)

ここで、設定にミスがないかを確認する場合は、ダミーデータを送信してみましょう。 `Connectivity Test`の`Try now`ボタンをクリックして`200 OK`と表示されることを確認してください。

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/c1d2ef3d-28a4-4441-99f3-9e026002b809)

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/722aeaa6-254f-49ee-9063-d0fc12fae32b)

Kintoneのアプリページを開き、入力したデータがアプリに追加されていることを確認してください。

`Finish`をクリックすると Cast が作成されます。

以上で連携作業は完了となります。

Castについて詳しい設定や仕様が知りたい場合には、Actcast Documents内の[Cast](https://actcast.io/docs/ja/ActManagement/Cast/) の項目をご確認ください。

 ## rate limitについて    
アクションが Webhook である Cast はレート制限の設定を持ちます。 レート制限の設定値とは Cast の URL が指す Cast 対象サービスに対して一定期間内に送出できるリクエストの回数の上限を表します。 Act に登録されているデバイスたちがこの上限以上の Act Log を送出しようとした場合、Actcast は上限を超える分を Cast 対象サービスにリクエストすることなく破棄します。
レート制限やその解除方法についてはActcast Documents内の[Castのレート制限](https://actcast.io/docs/ja/ActManagement/Cast/WebhookCastRateLimit/)に関する項目をご参照ください

## 免責

当社は、当社以外の第三者により管理・運営されるサービス（以下「第三者サービス」といいます。「Kintone」はこれに該当します。）と当社サービスをCastを用いて連携させる機能を提供していますが、その連携や連携の継続を保証するものではありません。

当社は、当社サービスと連携またはリンクされている第三者サービスの正確性、信頼性、完全性、有用性およびセキュリティにつき、いかなる保証もいたしません。第三者サービスの詳細や仕様、最新の情報については、第三者サービスの管理・運営者の公式サイト等を参照し、最新の情報を確認いただくことを強くお勧めします。

お客様は、第三者サービスの利用をお客様の責任において行うものとし、当社は第三者サービスの利用によりお客様又は第三者に生じた一切の損害について何らの責任を負いません。また、お客様は、第三者サービスの利用について当該第三者サービスの利用規約、契約等に従うものとし、自らの責任でこれらの内容を確認し、遵守してください。

Castを用いた当社サービスと第三者サービスとの連携に関して問題や疑問が生じたときは、当社までお問合せいただければ、可能な範囲でサポートさせていただきます。しかし、第三者サービスの使用方法やサービスに関するお問合せについては、第三者サービスの公式サポート窓口等をご利用いただきますようお願いいたします。