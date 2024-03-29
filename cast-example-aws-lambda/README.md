# AWS LambdaとのCast連携

## 概要

AWS（***Amazon Web Services***）はAmazonが提供するクラウドコンピューティングサービスの総称で、その中のサービスの一つに`AWS Lambda`があります。Lambdaはサーバーを用意することなくコードを実行できるサービスで、イベント駆動型のアプリケーションを簡単に構築できます。ActcastのCast機能と連携することで、Lambda上で様々な処理を実行することが可能です。また、Lambdaを使用することでAWS上の他の様々なクラウドリソースの呼び出しも簡潔に記述することができます。

## 利用コスト

詳細は[AWS公式ページ](https://aws.amazon.com/jp/lambda/pricing/)をご確認ください。

## 連携方法

AWSアカウントが必要になります。以下の手順に従って、実行する関数を作成します。

### AWS Lambdaの設定

1. AWSコンソールからLambda>関数>関数の作成をクリックします。

ここでは例としてランタイムにPython3.10を指定します

![untitled.png](https://github.com/Idein/actcast-tips/assets/106148688/1314e640-d366-4017-9f33-e97828ba4ecd)

2. コードソースを編集します。ここでは単純にリクエストに含まれる名前を返すコードを記述します。

```python
import json

def lambda_handler(body, context):
    # TODO implement
    name = body.get("name", "World")
    return {
        'statusCode': 200,
        'body': json.dumps(f"Hello {name}!")
    }

```

3. 設定→URLから「関数URLを作成」をクリックし、関数URLを発行します。

この際、関数URLの呼び出し権限の設定が求められます。今回は認証なし、即ちパブリックで作成します。

![untitled.png](https://github.com/Idein/actcast-tips/assets/106148688/cf41fe96-07e4-43b6-997e-e6d191bd2c7f)

![untitled.png](https://github.com/Idein/actcast-tips/assets/106148688/bd9ddac6-aece-49c9-9cb5-3218cbdcef89)

作成された関数URLをコピーしておきます。

### Cast設定

actcastにログイン後、左メニューから`任意のアプリケーション > Casts`を選択し、`Make New Cast`ボタンをクリックしてください。

![make_new_cast.png](https://github.com/Idein/actcast-tips/assets/106148688/70a480d3-0ee6-416c-a1d3-5da14eca9d3d)

まずはトリガーを設定します。 ここでは無条件ですべてのデータを送信させるために、何も設定せずに`Configure Action`をクリックします。

続いてアクションを設定します。

![cast_config.png](https://github.com/Idein/actcast-tips/assets/106148688/a71606c9-86b5-4413-897f-396dba884ec0)

`Webhook`を選択し、 Webhook 用の設定画面を表示します

![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/5434a5f6-d888-4e6b-ba40-523f0fc0ac09)

`URL`には先ほど作成した関数のエンドポイントを入力します

`Method`や`Headers`、`Body`に関しては、関数内で実装した内容に応じて設定します

`Finish`をクリックすると Cast が作成されます。

以上で連携作業は完了となります。

Castについて詳しい設定や仕様が知りたい場合には、Actcast Documents内の[Cast](https://actcast.io/docs/ja/ActManagement/Cast/) の項目をご確認ください。

 ## rate limitについて    
アクションが Webhook である Cast はレート制限の設定を持ちます。 レート制限の設定値とは Cast の URL が指す Cast 対象サービスに対して一定期間内に送出できるリクエストの回数の上限を表します。 Act に登録されているデバイスたちがこの上限以上の Act Log を送出しようとした場合、Actcast は上限を超える分を Cast 対象サービスにリクエストすることなく破棄します。
レート制限やその解除方法についてはActcast Documents内の[Castのレート制限](https://actcast.io/docs/ja/ActManagement/Cast/WebhookCastRateLimit/)に関する項目をご参照ください

## 免責

当社は、当社以外の第三者により管理・運営されるサービス（以下「第三者サービス」といいます。「AWS Lambda」はこれに該当します。）と当社サービスをCastを用いて連携させる機能を提供していますが、その連携や連携の継続を保証するものではありません。

当社は、当社サービスと連携またはリンクされている第三者サービスの正確性、信頼性、完全性、有用性およびセキュリティにつき、いかなる保証もいたしません。第三者サービスの詳細や仕様、最新の情報については、第三者サービスの管理・運営者の公式サイト等を参照し、最新の情報を確認いただくことを強くお勧めします。

お客様は、第三者サービスの利用をお客様の責任において行うものとし、当社は第三者サービスの利用によりお客様又は第三者に生じた一切の損害について何らの責任を負いません。また、お客様は、第三者サービスの利用について当該第三者サービスの利用規約、契約等に従うものとし、自らの責任でこれらの内容を確認し、遵守してください。

Castを用いた当社サービスと第三者サービスとの連携に関して問題や疑問が生じたときは、当社までお問合せいただければ、可能な範囲でサポートさせていただきます。しかし、第三者サービスの使用方法やサービスに関するお問合せについては、第三者サービスの公式サポート窓口等をご利用いただきますようお願いいたします。