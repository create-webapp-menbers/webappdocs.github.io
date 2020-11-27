Rust + Linux(ubuntu)環境作成メモ

前提条件
Windows insider program に登録済みであること
Windows buildバージョンが200x
Windowsロゴ → 検索バー表示 → 「Winver」入力でビルドバージョンを確認できる。

Dockerの使い方


---
title: Docker コマンドチートシート
tags: Docker docker-compose チートシート
author: wMETAw
slide: false
---
## よく使うコマンド

```bash

# 稼働中のコンテナに入る
$ docker-compose exec hoge_app bash

# stop
$ docker-compose stop

# docker-compose管理下のコンテナを削除し、紐づくボリュームも削除
$ docker-compose rm -v

# キャッシュを使わずにビルド
$ docker-compose build --no-cache

# コンテナを一括削除
$ docker rm `docker ps -a -q`

# 全てのボリュームを削除
$ docker volume rm $(docker volume ls -qf dangling=true)

# イメージ一覧
$ docker images -a

# イメージ一括削除
$ docker rmi `docker images -aq`
$ docker rmi -f `docker images -aq`

# docker images で REPOSITORY が <none> になっているものを削除（クライアントとデーモンAPIの両方がv1.25以降）
$ docker image prune

# docker images で REPOSITORY が <none> になっているものを削除（上記以下のバージョン）
$ docker rmi $(docker images | awk '/^<none>/ { print $3 }')

# ホストからコンテナへファイルをコピー
# docker cp <コピーするファイルのパス> <コンテナ名or ID>:<コンテナ保存先パス>
$ docker cp file.rc continername:/host/dir/ 

# コンテナからホストへファイルをコピー
# docker cp  <コンテナ名or ID>:<コピーするファイルのパス> <ホスト保存先パス>
$ docker cp  continer_name:/var/www/html/ /User/meta/hoge 
```

## コンテナの生成・起動

`$ docker run [オプション] [イメージ名:タグ名] [引数]`

| オプション    | 説明                       | 例                                              |
|---------------|----------------------------|-------------------------------------------------------|
| --name         | コンテナ名を指定           | docker run --name "test" centos                       |
| -d            | バッググラウンド実行       | docker run -d centos                                  |
| -it           | コンソールに結果を出力     | docker run -it --name "test" centos /bin/cal          |
| -p host:cont  | ポートフォワーディング     | docker run -d -p 8080:80 httpd                        |
| --add-host    | ホスト名とIPを指定         | docker run -it --add-host=test.com:192.168.1.1 centos |
| --dns         | DNSサーバを指定            | docker run --dns=192.168.1.1 httpd |
| --mac-address | MACアドレスを指定          | docker run -it --mac-address="92:d0..." centos |
| --cpu-shares  | CPU配分 (全体で1024)       | docker run --cpu-shares=512 centos                    |
| --memory      | メモリの上限               | docker run --memory=512m centos                       |
| -v            | ディレクトリの共有         | docker run -v /c/Users/src:/var/www/html httpd        |
| -e            | 環境変数を設定             | docker run -it -e foo=bar centos /bin/bash            |
| --env-file    | 環境変数リストから設定 | docker run -it --env-file=env_list centos /bin/bash   |
| -w            | 作業ディレクトリを指定     | docker run -it -w=/tmp/work centos /bin/bash          |

## 稼働中のコンテナ操作

| 説明                 | コマンド                                                  | 例(コンテナID=CID)                    |
|----------------------|-----------------------------------------------------------|-----------------------------------------|
| コンテナ一覧     | docker ps [オプション]                                    | docker ps                               |
| コンテナ確認       | docker stats コンテナID                                 | docker stats CID               |
| コンテナ起動       | docker start [オプション] コンテナID                      | docker start CID               |
| コンテナ停止       | docker stop [オプション] コンテナID                       | docker stop CID                |
| コンテナ再起動     | docker restart [オプション] コンテナID                    | docker restart CID             |
| コンテナ削除       | docker rm [オプション] コンテナID                         | docker rm CID                  |
| コンテナ中断       | docker pause コンテナID                                   | docker pause CID               |
| コンテナ再開       | docker unpause コンテナID                                 | docker unpause CID             |
| コンテナ接続       | docker attach コンテナID                                  | docker attach CID              |
| コンテナログ       | docker logs [オプション] コンテナID                         | docker logs CID              |
| プロセス実行         | docker exec [オプション] コンテナID コマンド [引数]       | docker exec -it CID /bin/cal   |
| プロセス確認         | docker top コンテナID                                     | docker top CID                 |
| ポート確認           | docker port コンテナID                                    | docker port CID                |
| コンテナ名変更       | docker rename 現在名 新しいコンテナ名                     | docker rename now new                   |
| イメージ差分         | docker diff コンテナID                                    | docker diff CID                |
| ファイルコピー       | docker cp コンテナID:ファイル パス                    | docker cp CID:/var/www/a.txt /Users/ryo/       |
| イメージの作成       | docker commit [オプション] コンテナID イメージ名[:タグ名] | docker commit CID my_image:1.0 |
| イメージを.tarへ出力        | docker export コンテナID                                  | docker export CID              |
| .tarからイメージ作成 | docker import パスorURL - イメージ名[:タグ名]             | cat src.tar｜docker import - web:1.0   |
| イメージ保存       | docker save [オプション] 保存ファイル名 [イメージ名]      | docker save -o src.tar web              |
| イメージロード     | docker load [オプション]                                  | docker load -i sec.tar                  |

補足
・ コンテナIDはコンテナ名でも可
・ `docker ps`は稼働中のコンテナ一覧。 `docker ps -a`は停止中を含めた全コンテナ一覧

【小技】

```
# 古いコンテナを一括削除
docker ps -a | grep 'weeks ago' | awk '{print $1}' | xargs docker rm

# コンテナを一括削除
docker rm `docker ps -a -q`
```

## イメージ操作
| 説明         | コマンド                                   | 例(イメージID=IID)             |
|--------------|--------------------------------------------|-----------------------------|
| pull | docker pull [オプション] イメージ名[:タグ] | docker pull centos:7        |
| 一覧 | docker images [オプション] [リポジトリ名]  | docker images -a            |
| 詳細 | docker inspect [オプション] [イメージID]   | docker inspect IID |
| 検索 | docker search [オプション] [キーワード]    | docker search nginx         |
| 削除 | docker rmi [オプション] イメージ名         | docker rmi nginx            |
| ログ | docker logs [オプション] イメージ名         | docker logs nginx            |
| タグ変更 | docker tag [イメージID] [リポジトリ名]:[タグ]  | docker tag IID hello-world:new_tag  |

【小技】

```
# イメージの一括削除
docker rmi `docker images -q`
```

## Docker Compose

| 説明               | コマンド                                              | 例                              |
|--------------------|-------------------------------------------------------|---------------------------------|
| コンテナ生成・起動 | docker-compose up [オプション] [サービス]             | docker-compose up -d            |
| 生成コンテナ数     | docker-compose scale [サービス=数]                    | docker-compose scale web=10     |
| コンテナ一覧       | docker-compose ps [オプション]                        | docker-compose ps               |
| コンテナログ       | docker-compose logs [オプション] [サービス]           | docker-compose logs             |
| コマンド実行       | docker-compose run [オプション] [サービス] [コマンド] | docker-compose run web /bin/cal |
| 全コンテナ起動     | docker-compose start [サービス]                       | docker-compose start            |
| 全コンテナ再起動   | docker-compose restart [オプション] [サービス]        | docker-compose restart          |
| 全コンテナ強制停止 | docker-compose kill [オプション] [サービス]           | docker-compose kill             |
| 全コンテナ削除     | docker-compose rm [オプション] [サービス]             | docker-compose rm               |
| 全コンテナをビルド     | docker-compose build [オプション]              | docker-compose build --no-cache  |



## DockerFile

| 説明 | コマンド | 例|
|--------------|--------------------------------------|----------------------------------|
|元となるイメージ |FROM | `FROM name/web:ver1.0`|
|作成者 | MAINTAINER | `MAINTAINER name`|
|環境変数 | ENV | ENV KEY=VALUE|
|指定のコマンドの実行 | RUN | `RUN yum -y install httpd`|
|イメージにファイル追加 | ADD | `ADD index.html /var/www/html/index.html`|
|ポート番号を指定 | EXPOSE | EXPOSE 3306|
|コンテナ起動時に実行するコマンド | CMD | CMD ["service","httpd","start"]|
|カレントディレクトリを指定 | WORKDIR | WORKDIR /var/www/html|
|ボリューム指定 | VOLUME | VOLUME /var/log/httpd|


- DockerFileは`命令 オプション`形式の行の羅列で構成される
- `#`でコメント
- Dockerfileは`build`サブコマンドでのビルド時に利用される
- Dockerのビルドは、コマンド単位をステップとして実行する
  - 各ステップはコンテナのスナップショットをキャッシュする
  - Dockerfileを部分変更した場合は、その差分のみが実行される


## Docker Hub

| 説明         | コマンド                             | 例                         |
|--------------|--------------------------------------|----------------------------------|
| ログイン     | docker login [オプション] [サーバー] | docker login -u "name" -p "pass" |
| ログアウト   | docker logout [サーバ名]             | docker logout                    |
| アップロード | docker push ユーザ名/イメージ名[:タグ名]      | docker push user/my_image:v1.0        |


```bash
    # Dockerhubへログイン
    docker login -u "name" -p "pass"

    # カレントディレクトリのDockerfileからイメージを作成 
    # docker build -t ユーザー名/イメージ名:タグ名 .
    docker build -t wmetaw/mylinux:1.0 .

    # Dockerhubへpush
    docker push wmetaw/mylinux:1.0
```

## Docker本体の情報

| 説明           | コマンド       |
|----------------|----------------|
| バージョン確認 | docker version |
| 実行環境確認   | docker info    |

## Dockerコンポーネント
Dockerはコア機能を提供する「Docker Engine」を中心に「イメージの作成→公開→コンテナ実行」を行うためのコンポーネント（部品）が提供されています
主なコンポーネントは次の通り

<hr>

#### **Docker Engine（Dockerのコア機能）**
Docker イメージの生成やコンテナの軌道などを行うためのコア機能
Dockerコマンドの実行やDockerFileによるイメージの生成を行う
<br><hr>

#### **Docker Machine（Docker実行環境構築）**
VirtualBoxをはじめ、AWSやAZUREなどのクラウド環境などにDockerの実行環境をコマンドで自動生成するツール
<br><hr>

#### **Docker Compose（複数コンテナの一元管理）**
複数コンテナの構成情報をコードで定義して、コマンドを実行することでアプリケーションの実行環境を構成するコンテナ群を一元管理するためのツール
<br><hr>

#### **Docker Swarm（クラスタ管理）**
Docker Swarmは複数のホストをクラスタ化するためのツール
クラスタの管理やAPIの提供を行う役割が「Manager」、Dockerコンテナを実行する役割が「Node」という
<br><hr>

#### **Docker Kitematic（DockerのGUIツール）**
Dockerイメージの生成やコンテナの起動などを行うためのGUIツール
グラフィカルなUIなので非エンジニアの方に最適
<br><hr>

#### **Docker Registry（イメージの公開・共有）**
コンテナの元となるイメージを公開・共有する為のレジストリ機能
Docker公式のレジストリサービスであるDocker Hubもこの機能を使用している
<br><hr>

#### **[Docker Hub](https://hub.docker.com/)（Docker公式のレジストリ）**
CentOS・nginxなどの公式イメージはこのレジストリから取得する
publicなので多くのユーザーによる自作のイメージが公開されている
