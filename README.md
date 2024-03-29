# sshmail-skinned
メールサーバがssh経由でしかアクセスできない場所にあって、ローカルのメーラーを使いたくないときに使えるdockerイメージ群。
webメールクライアントを使いたいていう動機。

自分の環境に合わせて作ってあるので設定が合わなかったらテキトーにいじると何とかなる。

## Usage
`$ ./startup.sh`で開始。

`docker-compose down`で停止。

## Env
- docker-compose version 1.28.5, build c4eb3a1f
- Docker on WSL2
- chrome

## Settings
.envファイルを編集し、接続する$remote$と$target$を設定する。
(.env.sampleを参考にいい感じにする。)

初回起動時は、 http://localhost:8888/?/admin にアクセス、初期設定をテキトウに行う。(user:admin, password:12345)
- ドメイン->ドメインを追加から、新しいドメインを登録する。
  - 名前のところに、追加したいドメインを書く
  - IMAPサーバーは`imap-ambassador`SMTPサーバーは`smtp-ambassador`とする。(1つ入れると両方入る)
  - セキュリティは両方 SSL/TLS にする。
  - 「短いログイン名を使う」にチェック。
  - ポートはいじらなくてOK
  - 接続テスト押してペカったら完了

http://localhost:8888/ にアクセスして、メアドとパスワードでログインする。->使える!

## Tips
- 一回立てると設定は永続化される。
- 永続化のためのボリュームファイルは、Linux環境ではownerの違いで削除にはsudoが必要。
- 秘密鍵のロケーションを設定しているが、イメージに含まれることはなく、起動時に都度マウントされる。

## Roadmap
- DBのコンテナと、ボリュームコンテナを追加したい。
- 設定ファイルのパーミッション問題を直したい
  - (Docker on Linuxでは既知のissue)
  - 回避法も色々。。。
- 設定ファイル(application.ini,domein.ini)が存在するので、初回起動時に自動で環境変数から設定を書き出せるようにしたい
  - たぶんできそう。

## おまけ
設定するとHTTPも自動でつながる。
