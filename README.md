# AWS WAFを学びたいので、以下のハンズオンを実施する会

- https://pages.awscloud.com/JAPAN-event-OE-Hands-on-for-Beginners-CF_WAF-2022-confirmation_129.html
- https://docs.aws.amazon.com/waf/latest/developerguide/getting-started.html

## Cloud Frontの設定項目

- オリジン
  - 対象となるリソースを指定する
  - 例えばS3やAPI Gateway(lambda)などが挙げられる
  - S3などの静的コンテンツの場合はキャッシュを設定し、lambdaなどの場合はキャッシュしない設定をする
- ビヘイビア
  - 特定のパスにアクセスがあった際にキャッシュするかしないかなどを設定する
  - ここでオリジンを指定
  - キャッシュポリシー欄でCachingDisableやCachingOptimizedなどを選ぶ
  - API Gatewayに対してクエリパラメータを送るためにはオリジンリクエストポリシーを設定する必要がある
- ポリシー
  - キャッシュポリシーやオリジンリクエストポリシーは自分で作成することができ、それをビヘイビアに設定することができる
  - キャッシュポリシーではTTL設定やキャッシュキー設定が可能
  - オリジンリクエストポリシーではオリジンリクエストの設定が可能

## Cloud FrontでS3をオリジンとしたCDNを実装する

Cloud Frontの設定で403が起こった

- S3の静的WebサイトホスティングがOFFになっていた
  - ONにする必要がある
- アクセス許可でパブリックアクセスをすべてブロックがオンになっていた
  - OFFにする必要がある
- バケットポリシーが適切に設定されていなかった
  - 以下のように設定する必要がある

```policy.json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::[bucket-name]/*"
        }
    ]
}
```
