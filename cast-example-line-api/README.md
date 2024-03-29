# Line APIとのCast連携

## **概要**

LINE Notifyは、LINEヤフー株式会社が提供するサービスであり、特定のイベントや条件に基づいて、LINEアプリ内のLine Notifyを通じて通知を送ることができます。

LINE Notifyに関する最新の情報や詳細については、以下の公式URLをご参照ください。

[LINE Notify公式サイト](https://notify-bot.line.me/ja/)

Line Notifyと連携することにより、任意の内容をCast機能を通じて送信することができます

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/a89af82f-c266-432c-b8ba-605329e7fd3a)

## 制約(2024/04/01現在)

| 特徴 | Line Notify |
| --- | --- |
| 画像送信 | 不可 |
| 広告・販売促進目的での利用 | 不可 |
| 送信上限 | 1hour/1000 |
| 料金 | 無料 |

## 連携方法

[LINE Notify](https://notify-bot.line.me/ja/)

LINE Notifyのページからログインを行って、以下のサイトへ遷移してください。

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/f56e5edf-b7e7-46d4-ad93-b78c0a4c171c)


トークンを発行するから生成ページへと移動、通知を送信したいトークルーム等の設定を行い、発行されたトークンを控えてください。

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/a15e21e6-9dc5-449d-8f62-de54f922c521)


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
URL:https://notify-api.line.me/api/notify
Method:POST
Headers:Content-Type:application/x-www-form-urlencoded
Authorization:Bearer 【YourToken】
Body:message=hogehoge
```

【YourToken】には先程生成したトークンを入力してください。

入力が完了すると、以下の図のような状態になります。

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/c132f8fe-932b-4ac8-8822-a0cce331387f)


ここで、設定にミスがないかを確認する場合は、ダミーデータを送信してみましょう。 `Connectivity Test`の`Try now`ボタンをクリックして`200 OK`と表示されれば OK です。

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/c1d2ef3d-28a4-4441-99f3-9e026002b809)


`Finish`をクリックすると Cast が作成されます。

以上で連携作業は完了となります。

Castについて詳しい設定や仕様が知りたい場合には、Actcast Documents内の[Cast](https://actcast.io/docs/ja/ActManagement/Cast/) の項目をご確認ください。

 ## rate limitについて    
アクションが Webhook である Cast はレート制限の設定を持ちます。 レート制限の設定値とは Cast の URL が指す Cast 対象サービスに対して一定期間内に送出できるリクエストの回数の上限を表します。 Act に登録されているデバイスたちがこの上限以上の Act Log を送出しようとした場合、Actcast は上限を超える分を Cast 対象サービスにリクエストすることなく破棄します。
レート制限やその解除方法についてはActcast Documents内の[Castのレート制限](https://actcast.io/docs/ja/ActManagement/Cast/WebhookCastRateLimit/)に関する項目をご参照ください

## 免責

当社は、当社以外の第三者により管理・運営されるサービス（以下「第三者サービス」といいます。「LINE Notify」はこれに該当します。）と当社サービスをCastを用いて連携させる機能を提供していますが、その連携や連携の継続を保証するものではありません。

当社は、当社サービスと連携またはリンクされている第三者サービスの正確性、信頼性、完全性、有用性およびセキュリティにつき、いかなる保証もいたしません。第三者サービスの詳細や仕様、最新の情報については、第三者サービスの管理・運営者の公式サイト等を参照し、最新の情報を確認いただくことを強くお勧めします。

お客様は、第三者サービスの利用をお客様の責任において行うものとし、当社は第三者サービスの利用によりお客様又は第三者に生じた一切の損害について何らの責任を負いません。また、お客様は、第三者サービスの利用について当該第三者サービスの利用規約、契約等に従うものとし、自らの責任でこれらの内容を確認し、遵守してください。

Castを用いた当社サービスと第三者サービスとの連携に関して問題や疑問が生じたときは、当社までお問合せいただければ、可能な範囲でサポートさせていただきます。しかし、第三者サービスの使用方法やサービスに関するお問合せについては、第三者サービスの公式サポート窓口等をご利用いただきますようお願いいたします。

