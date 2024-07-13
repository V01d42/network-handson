# Network Handson
ネットワーク設定についてのハンズオン

## Getting Started

本ハンズオンではContainerlabを用いたネットワーク検証環境の構築を行います。そのため以下のLinux(WSL2)環境が必要です。
* [build-essential](https://docs.docker.com/engine/install/) - Vrnetlabにてmakeする必要があるためそのための主要コンパイラのパッケージ
* [Docker CE](https://docs.docker.com/engine/install/) - Containerlabを動かすためのコンテナ環境
* [Containerlab](https://containerlab.dev/) - コンテナベースのネットワークラボを構築してくれるアプリケーション
* [Vrnetlab](https://github.com/hellt/vrnetlab) - VMベースの仮想ルータイメージをコンテナ化するアプリケーション

### Prerequisites

ContainerlabはDocker Desktopでは動かないためDocker CEがインストールされているかを確認する必要があり、以下のコマンドで確認可能。
```
apt list | grep docker-ce
```

### Installing

#### Clone repository
```
git clone [this repository URL]
cd network_handson
```

#### build-essential
```
sudo apt update
sudo apt upgrade
sudo apt install build-essential
```

#### Docker CE
注）Docker CEがインストールされている場合は飛ばしてContainerlabの項に進んでください。
```
sudo apt -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### Containerlab

```
bash -c "$(curl -sL https://get.containerlab.dev)"
```

#### Vrnetlab

```
cd containerlab
git clone https://github.com/hellt/vrnetlab && cd vrnetlab
```

<!-- 
## Running the tests

自動テストをどのように実行するかをここで説明します

### Break down into end to end tests

何のためのテストでどうしてそれを行うのかを説明します

```
コマンドで例を示します
```

### And coding style tests

何のためのテストでどうしてそれを行うのかを説明します

```
コマンドで例を示します
```
-->

## Deployment

一部Publicにfetchできないimageが存在するため、VMバイナリからVrnetlabを用いてimageを作成する。

### make juniper_vjunosrouter container(Vrnetlab)

Juniper公式の[vjunosrouterのVM配布サイト](https://support.juniper.net/support/downloads/?p=vjunos-router)にアクセスし、`Application Package`の`Downloads`にある`qcow2`をクリックしてlicenseに同意、表示されたURLをコピーして、以下のコマンドを実行

```
cd vjunosrouter
curl -L -o vJunos-router-23.2R1.15.qcow2 <コピーしたURL>
make
```

実行したのち`docker image ls`にて`vrnetlab/vr-vjunosrouter`というimageが作成されていれば成功

これを[vjunosswitchのVM配布サイト](https://support.juniper.net/support/downloads/?p=vjunos-switch)でも同様に行う。

### build and destroy network

#### build network
以下のコマンドで`configs/demo1.clab.yml`のトポロジーのネットワークを構築できる。
```
sudo containerlab deploy -t configs/demo1.clab.yml
```

実行後以下のように表示されれば成功
```
+---+-------------------------+--------------+------------------------------------+----------------------+---------+---------------+----------------------+
| # |          Name           | Container ID |               Image                |         Kind         |  State  |  IPv4 Address  |     IPv6 Address     |
+---+-------------------------+--------------+------------------------------------+----------------------+---------+---------------+----------------------+
| 1 | clab-demo1-client       | xxxxxxxxxxxx | ghcr.io/hellt/network-multitool    | linux                | running | xxx.xx.xx.x/xx | xxxx:xxx:xx:xx::x/xx |
| 2 | clab-demo1-vjunosrouter | xxxxxxxxxxxx | vrnetlab/vr-vjunosrouter:23.2R1.15 | juniper_vjunosrouter | running | xxx.xx.xx.x/xx | xxxx:xxx:xx:xx::x/xx |
+---+-------------------------+--------------+------------------------------------+----------------------+---------+---------------+----------------------+
```
#### login VMs
以下のコマンドでそのコンテナにログインをできる。
```
docker exec -it <container-name/id> bash
```

またvJunos-switchもしくはvJunos-routerには以下のコマンドを用いてsshでアクセスすることができる。
ログイン時のpasswordは`admin@123`である。
```
ssh admin@<container-name/id>
```

#### destroy network
以下のコマンドで`configs/demo1.clab.yml`のトポロジーのネットワークを削除できる。
```
sudo containerlab destroy -t configs/demo1.clab.yml
```

<!-- 
## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

私たちのコーディング規範とプルリクエストの手順についての詳細は[CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426)を参照してください。

## Versioning

私たちはバージョン管理に[SemVer](http://semver.org/)を使用しています。利用可能なバージョンは[このリポジトリのタグ](https://github.com/your/project/tags)を参照してください。

## Authors

* **John Smith** - *Initial work* - [Hoge](https://github.com/hoge)

このプロジェクトへの[貢献者](https://github.com/your/project/contributors)のリストもご覧ください。

## License

このプロジェクトは MIT ライセンスの元にライセンスされています。 詳細は[LICENSE.md](LICENSE.md)をご覧ください。

## Acknowledgments

* コードを書いた人への感謝
* 何からインスピレーションを得たか
* その他
-->
