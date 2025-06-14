<!-- omit in toc -->
# Multipass::Overview

<!-- omit in toc -->
## TOC

- [概要](#概要)
  - [特徴](#特徴)
- [インストール](#インストール)
- [使い方](#使い方)
  - [インスタンス起動(作成)、停止、削除の基本操作](#インスタンス起動作成停止削除の基本操作)
    - [最新のLTSでインスタンス起動](#最新のltsでインスタンス起動)
    - [インスタンスの停止](#インスタンスの停止)
    - [インスタンス起動](#インスタンス起動)
    - [インスタンス削除](#インスタンス削除)
  - [VMインスタンスへのログイン](#vmインスタンスへのログイン)
  - [VMインスタンスのリソース変更](#vmインスタンスのリソース変更)
    - [VMインスタンス起動(作成)時の変更](#vmインスタンス起動作成時の変更)
    - [停止したVMインスタンスでの変更](#停止したvmインスタンスでの変更)
  - [cloud-initを使ったVMインスタンス起動](#cloud-initを使ったvmインスタンス起動)
  - [ホストマシンのディレクトリをマウント](#ホストマシンのディレクトリをマウント)
- [コマンドリファレンス](#コマンドリファレンス)
  - [使用できるイメージ一覧](#使用できるイメージ一覧)
  - [イメージを指定してインスタンスを起動](#イメージを指定してインスタンスを起動)
  - [設定パラメータの確認](#設定パラメータの確認)
- [参考リンク](#参考リンク)

## 概要

Canonical社が開発しているハイパーバイザ型VM管理ソフト。  
コマンド1つで、Ubuntu VMを起動することができる。  

### 特徴

- クロスプラットフォーム(Windows, macOS, Linux)
- cloud-initが使える
- CLIインターフェイス

## インストール

(macOS)

```bash
$ brew install --cask multipass
```

## 使い方

### インスタンス起動(作成)、停止、削除の基本操作

#### 最新のLTSでインスタンス起動

```bash
$ multipass launch --name ubuntu-lts   # ubuntu-ltsという名前でインスタンスを起動
$ multipass list   # インスタンス一覧を表示
Name                    State             IPv4              Image
ubuntu-lts              Running           XXX.XXX.XXX.XXX   Ubuntu 24.04 LTS
```

#### インスタンスの停止  

起動したインスタンスの停止

```bash
$ multipass stop ubuntu-lts   # インスタンス停止
$ multipass list   # 停止したことを確認
Name                    State             IPv4             Image
ubuntu-lts              Stopped           --               Ubuntu 24.04 LTS
```

#### インスタンス起動

停止したインスタンスの起動

```bash
$ multipass start ubuntu-lts   # インスタンス起動
$ multipass list
Name                    State             IPv4              Image
ubuntu-lts              Running           XXX.XXX.XXX.XXX   Ubuntu 24.04 LTS
```

#### インスタンス削除

```bash
$ multipass delete ubuntu-lts   # 対象インスタンスを削除対象にする
$ multipass list   # StateがDeletedに変更される
Name                    State             IPv4             Image
ubuntu-lts              Deleted           --               Ubuntu 24.04 LTS
$ multipass purge   # 完全削除(StateがDeletedなインスタンスを全て削除)
```

削除対象にしたVMインスタンスを復元  

```bash
$ multipass recover ubuntu-lts   # 対象インスタンスを削除対象から復元
$ % multipass list   # StateがStoppedに変更される
Name                    State             IPv4             Image
ubuntu-lts              Stopped           --               Ubuntu 24.04 LTS
```

### VMインスタンスへのログイン

```bash
$ multipass shell ubuntu-lts
```

### VMインスタンスのリソース変更

VMインスタンスはデフォルトだと、以下のリソースで起動する。  

```
* CPU : 1core
* RAM : 1.0GiB
* DISK : 5.0GiB
```

#### VMインスタンス起動(作成)時の変更

```bash
$ multipass launch --name ubuntu-lts --cpus 2 --memory 2.0G --disk 10G
```

#### 停止したVMインスタンスでの変更

```bash
$ multipass set local.ubuntu-lts.cpus=2   # CPUを2コアに変更
$ multipass set local.ubuntu-lts.memory=2.0GB   # RAMを2GBに変更
$ multipass set local.ubuntu-lts.disk=10GB   # DISKを10GBに変更
```

### cloud-initを使ったVMインスタンス起動

VMインスタンス起動時に、cloud-initを使って、インスタンスを初期化できる。  
  
e.g. Nginxをセットアップ  

```yaml:cloud-init.yml
#cloud-config
repo_update: true
repo_upgrade: all

packages:
  - nginx

runcmd:
  - systemctl start nginx.service
  - systemctl enable nginx.service
```

`--cloud-init` オプションでcloud-init yamlファイルを指定して、起動  

```bash
$ multipass launch --name ubuntu-lts --cloud-init cloud-init.yml
$ multipass list
Name                    State             IPv4             Image
ubuntu-lts              Running           XXX.XXX.XXX.XXX     Ubuntu 24.04 LTS
$ curl -i http://XXX.XXX.XXX.XXX
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
(以下、省略)
```

### ホストマシンのディレクトリをマウント

```bash
$ multipass mount /Work/to/path/share-dir ubuntu-lts:/home/ubuntu/share   # ホストマシンの/Work/to/path/share-dirディレクトリをVMインスタンスの/home/ubuntuにマウント
$ multipass info ubuntu-lts | grep -i 'mounts:'
Mounts:         /Work/to/path/share-dir => /home/ubuntu/share
```

## コマンドリファレンス

### 使用できるイメージ一覧

```bash
$ multipass find
```

### イメージを指定してインスタンスを起動

```bash
$ multipass launch 24.10 --name ubuntu-oracular   # Ubuntu24.10なインスタンスを起動
```

### 設定パラメータの確認

```bash
$ multipass get --keys   # 確認できるパラメータキー一覧

(e.g.)
$ multipass get local.driver   # ハイパーバイザのバックエンド確認
qemu 
```

## 参考リンク

- [Multipass](https://canonical.com/multipass)
- [Multipass Document](https://documentation.ubuntu.com/multipass/en/latest/)
- [cloud-init](https://cloudinit.readthedocs.io/en/latest/index.html)

