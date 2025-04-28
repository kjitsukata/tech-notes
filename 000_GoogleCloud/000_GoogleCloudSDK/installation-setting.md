# Google Cloud SDK > Installation and Setting

## Installation

### Homebrewによるインストール

```
% brew install google-cloud-sdk
```

### Homebrew以外のインストール(インストール公式ドキュメント)

[Install the gcloud CLI](https://cloud.google.com/sdk/docs/install#installation_instructions)

### バージョン確認・アップデート・コンポーネントインストール

```
## バージョン確認
% gcloud version
Google Cloud SDK 519.0.0
bq 2.1.15
core 2025.04.18
gcloud-crc32c 1.0.0
gsutil 5.34

## 最新バージョンにアップデート
% gcloud components update

## 全てのコンポーネントを表示
% gcloud components list

## コンポーネントをインストール
% gcloud components install COMPONENT_ID

## コンポーネントの削除
% gcloud components remove COMPONENT_ID
```

## Setting

### 初期設定

初期状態で作成されているdefault設定にアカウント認証などを実施していきます。  

```
% gcloud init
.....
You must sign in to continue. Would you like to sign in (Y/n)?  Y
.....
```

ブラウザが起動し、ログイン画面が表示されるので、プロジェクトを利用するGoogleアカウントでログインします。  
その後、プロジェクトの選択やデフォルトゾーンの選択を実施します。  

### default以外の設定を作成

```
## 設定確認
% gcloud config configurations list

## 設定作成
% gcloud config configurations create my-project --no-activate

## 設定をアクティベート
% gcloud config configurations activate MY-PROJECT
Activated [MY-PROJECT].

## アカウント認証
## URLが表示されるので、ブラウザでアクセスしアカウント認証をします。ブラウザにverification codeが表示されるので、ターミナルにペーストします。
% gcloud auth login --no-launch-browser
.....
Once finished, enter the verification code provided in your browser: [ブラウザに表示されたverification code]

## プロジェクト設定
% gcloud config set project PROJECT_ID

## デフォルトリージョン設定
% gcloud config set compute/region REGION_NAME

## デフォルトゾーン設定
% gcloud config set compute/zone ZONE_NAME

## 設定値確認
% gcloud config list
## 設定値確認(明示的に設定していない設定含め、全ての設定を表示)
% gcloud config list --all
```

設定を切り替える場合は、gcloud config configurations activateコマンドを使用します。  

## Other command

```
## 認証したアカウント表示
% gcloud auth list

## service accountでgcloud認証
% gcloud auth activate-service-account SERVICE_ACCOUNT --key-file=SERVICE_ACCOUNT_KEY_FILE_PATH

## アカウントのOAuth2.0アクセストークン出力
% gcloud auth print-access-token
## 指定したサービスアカウントのOAuth2.0アクセストークン出力
% gcloud auth print-access-token --impersonate-service-account=SERVICE_ACCOUNT

## アカウントのOpenID Connect(OIDC) IDトークン出力
% gcloud auth print-identity-token
## 指定したサービスアカウントのOpenID Connect(OIDC) IDトークン出力
% gcloud auth print-identity-token --impersonate-service-account=SERVICE_ACCOUNT
```

## Reference

- [Authentication methods at Google](https://cloud.google.com/docs/authentication)
- [gcloud config list](https://cloud.google.com/sdk/gcloud/reference/config/list)
- [gcloud config set](https://cloud.google.com/sdk/gcloud/reference/config/set)

