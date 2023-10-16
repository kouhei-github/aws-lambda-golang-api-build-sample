# AWS LambdaでGolangでAPIサーバーを構築
技術スタック
AWS Lambda
Lambda Function URL
Golang
Golang(net/http)

---

## 1. 動かすために必要なこと

### 1.1 .envファイルの作成
```shell
cp .env.sample .env
```

・Lineでアカウントを作成して、チャンネルシークレットとアクセストークをセットする<br>
・OpenAIのAPIキーをセットする
```text
LINE_CHANNEL_SECRET=
LINE_CHANNEL_ACCESS_TOKEN=

OPENAI_API_KEY=
```
---

## 2. 起動・停止方法
### 2.1 起動方法
imageの作成
```shell
docker compose build
```

imageからコンテナの起動
```shell
docker compose up -d
```

---

### 2.2 停止方法
```shell
docker compose down
```

---

### 3. デプロイ方法
```shell
docker compose -f docker-compose-prod.yml build
aws ecr get-login-password --region ap-northeast-1 --profile={プロファイル名} --region ap-northeast-1  | docker login --username AWS --password-stdin {account_id}.dkr.ecr.ap-northeast-1.amazonaws.com
docker tag line-fortune-telling-prod-image:latest {account_id}.dkr.ecr.ap-northeast-1.amazonaws.com/line-fortune-telling-prod-image:latest
docker push {account_id}.dkr.ecr.ap-northeast-1.amazonaws.com/line-fortune-telling-prod-image:latest
aws lambda update-function-code --function-name {Lambda関数名} --image-uri {account_id}.dkr.ecr.ap-northeast-1.amazonaws.com/line-fortune-telling-prod-image:latest --profile={プロファイル名}
```
