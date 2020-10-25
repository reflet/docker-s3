# AWS S3環境構築 (minio)
ローカルでAWS S3環境を構築する。

## ■前提条件
* macOS環境
* [docker](https://docs.docker.com/get-docker/) をインストールしていること
* [docker-compose](https://docs.docker.com/compose/install/) をインストールしていること

## ■コード配置
必要となるコードをリポジトリからcloneして配置します。

```
$ mkdir -p ~/workspace/docker-s3 && cd ~/workspace/docker-s3
$ git clone https://github.com/reflet/docker-s3.git .
```

## ■Dockerイメージ作成 & 起動する
下記コマンドにて、dockerイメージを作成して、コンテナを起動します。

```
$ docker-compose build --no-cache
$ docker-compose up -d
```

## ■S3管理画面
下記のようにブラウザでアクセスする。

```
$ open http://127.0.0.1:9090/
```

## ■S3に登録したファイルについて
下記のようにブラウザでアクセスする。

例) hoge.txt, 150x150.png, 640x480.png
```
$ open http://127.0.0.1:9090/static/hoge.txt
$ open http://localhost:9090/static/150x150.png
$ open http://localhost:9090/static/640x480.png
```

## ■その他
aws-cliで操作する場合、下記のように操作できます。

### 環境設定
minioで作成するS3環境へaws-cliコマンドでアクセスするためのプロフィールを設定する。

```
$ aws configure --profile minio
AWS Access Key ID [None]: minio
AWS Secret Access Key [None]: minio123
Default region name [None]: ap-northeast-1
Default output format [None]: json

$ aws configure list --profile minio
```

### バケットを作成する
`static` バケットを作る例です。

```
$ aws --profile minio --endpoint-url http://127.0.0.1:9090/ s3 mb s3://static

$ aws --profile minio --endpoint-url http://127.0.0.1:9090/ s3 ls
2020-10-25 09:20:33 static
```

### ファイルを追加する
`hoge.txt` を `static` バケットに追加する例です。

```
$ aws --profile minio --endpoint-url http://127.0.0.1:9090/ s3 cp hoge.txt s3://static
upload: ./hoge.txt to s3://static/hoge.txt

$ aws --profile minio --endpoint-url http://127.0.0.1:9090/ s3 ls s3://static
2020-10-25 09:33:53          5 hoge.txt
```

### ファイルを削除する
`hoge.txt` を `static` バケットから削除する例です。

```
$ aws --profile minio --endpoint-url http://127.0.0.1:9090/ s3 rm s3://static/hoge.txt
delete: s3://static/hoge.txt
```

## 参考サイト
- https://hub.docker.com/r/minio/minio/
- https://docs.min.io/docs/deploy-minio-on-docker-compose.html
- https://qiita.com/daisukeArk/items/ac97665e5de7b726fa67
- https://gist.github.com/kanolato/577d6329ea5107826c7230bab59f1f02

以上
