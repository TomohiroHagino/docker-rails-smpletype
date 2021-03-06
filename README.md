# Docker導入パッケージ、シングルタイプ

# サービスに関して

```
設定のダルさを極限まで抑えつつRailsプロジェクトをDocker化できます。

rails newで作ることもできるようにしました。

こちらは、コンテナ単体版。

```


# RailsのGitリポジトリをクローンして、すぐにDocker化しやすくするファイル群

```
git cloneしてきて、３コマンドくらい打って、docker-compose upとすれば起動できます！
```


# 使用方法

```
0.このリポジトリをクローンして.gitファイルを削除する。
(mac,linuxの場合)
$ rm -rf .git
(windowsの場合)
> del .git

1. docker desktopをインストールしたあと、このDocker用のファイル郡とおなじ階層にRailsのリポジトリをgit cloneしてください。
(mac,linux,windows共通)
git clone <クローンしたいRailsリポジトリ>

(mac,linux)
$ mv <クローンしたいRailsリポジトリ名>/* .
(windows)
> move <クローンしたいRailsリポジトリ名>/* .

(もし新規作成するなら、 rails new <プロジェクト名> --skip-test)
(mac,linux)
(mv <プロジェクト名>/* .)
(windows)
(move <プロジェクト名>/* .)


2. そして、/docker/railsディレクトリに入っているDockerfileの1行目にかかれたRubyのバージョンをお好みのものに変更する。

3. そして、/docker/railsディレクトリに入っているGemfileをクローンしたプロジェクトのGemfileで上書きする。Gemfile.lockは空っぽにする。
(mac,linux)
$ cp Gemfile ./docker/rails/
$ cp Gemfile.lock ./docker/rails/
(windowsの場合)
> copy Gemfile ./docker/rails/
> copy Gemfile.lock ./docker/rails/

(Railsのバージョンは5、開発時はsqlite3、本番時はpostgresqlを使用するのを想定してます。)
(Rails6をインストールしたい場合はDockerfileとentrypoint.shのyarnのコメントアウトを外すこと)

4. ここから環境構築。初回時はこれ（imageがなければimageビルドから）コンテナの起動までを行う。
(mac,linux,windows共通)
$ docker-compose up -d

railsコンテナのターミナルに直接アクセス
(mac,linux,windows共通)
$ docker-compose exec rails ash

5. サーバーの立ち上げ
#(コンテナ内で) rails s -b 0.0.0.0

6. DB関連 コンテナに直接アクセスして以下のコマンドをやればOK
#(コンテナ内で) rails db:reset
#(コンテナ内で) rails db:create
#(コンテナ内で) rails db:migrate
#(コンテナ内で) rails s -b 0.0.0.0

作業を終了する時はコンテナを終了させます。
$ docker-compose stop

2回目以降からは以前に作ったコンテナを起動させます。
$ docker-compose start

```

