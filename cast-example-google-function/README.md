# Google FunctionsとのCast連携

## 概要

GCP（***Google Cloud Platform*** ）はGoogleが提供するクラウドコンピューティングサービスの総称で、その中のサービスの一つに***Cloud Functions***があります。Cloud Functionsはサーバーやコンテナによる管理なしでクラウド上でコードを実行することができるサービスです。ActcastのCast機能と連携することで、Cloud Functions上で様々な処理を実行することができます。

## 利用コスト

詳細は[公式サイト](https://cloud.google.com/functions/pricing?hl=ja)をご確認ください。

## 連携方法

GCPのアカウントが必要になります。[Cloud Functions](https://cloud.google.com/functions?hl=ja)にアクセスし、任意のコンソールへ移動した後、以下の手順にしたがって、実行する関数を作成します。

### Cloud Functionsの設定

1. ファンクションを作成
    
    ![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/25669530-28da-4379-91f3-e9710de76570)
    
2. 基本情報を入力
    
    <img width="406" alt="Untitled (1)" src="https://github.com/Idein/actcast-tips/assets/106148688/c17a7957-aa4e-4ae1-8e2f-68635baa8ee5">
    
3. ランタイムや接続等の設定
    
    ![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/e46b3934-d5e3-46f7-b620-69bc7163ce98)
    
4. 次へをクリック
5. 関数の実装
    
    任意の言語を選択し、main関数内に処理を実装します
    
    ![Untitled](https://github.com/Idein/actcast-tips/assets/106148688/31c40c57-118a-47d7-b549-fc443c748548)
    
6. デプロイをクリック
7. トリガータブからエンドポイントを取得
    
    <img width="499" alt="Untitled (4)" src="https://github.com/Idein/actcast-tips/assets/106148688/2730ecdc-31df-4d45-8429-fdceb47bc8f4">
    

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

当社は、当社以外の第三者により管理・運営されるサービス（以下「第三者サービス」といいます。「GCP Cloud Functions」はこれに該当します。）と当社サービスをCastを用いて連携させる機能を提供していますが、その連携や連携の継続を保証するものではありません。

当社は、当社サービスと連携またはリンクされている第三者サービスの正確性、信頼性、完全性、有用性およびセキュリティにつき、いかなる保証もいたしません。第三者サービスの詳細や仕様、最新の情報については、第三者サービスの管理・運営者の公式サイト等を参照し、最新の情報を確認いただくことを強くお勧めします。

お客様は、第三者サービスの利用をお客様の責任において行うものとし、当社は第三者サービスの利用によりお客様又は第三者に生じた一切の損害について何らの責任を負いません。また、お客様は、第三者サービスの利用について当該第三者サービスの利用規約、契約等に従うものとし、自らの責任でこれらの内容を確認し、遵守してください。

Castを用いた当社サービスと第三者サービスとの連携に関して問題や疑問が生じたときは、当社までお問合せいただければ、可能な範囲でサポートさせていただきます。しかし、第三者サービスの使用方法やサービスに関するお問合せについては、第三者サービスの公式サポート窓口等をご利用いただきますようお願いいたします。