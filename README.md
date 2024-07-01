# AWS WAFを学びたいので、以下のハンズオンを実施する会

- https://pages.awscloud.com/JAPAN-event-OE-Hands-on-for-Beginners-CF_WAF-2022-confirmation_129.html
- https://docs.aws.amazon.com/waf/latest/developerguide/getting-started.html

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
