# VirtualBoxとVagrantでRuby on Rails開発環境を構築

## 概要

https://github.com/rails/rails-dev-box の日本語版です。  
このプロジェクトはRuby on Railsの開発環境を自動で構築します。  
このVMを使えば、開発からテストまで全ての準備が整います。

## 必要なもの

* [VirtualBox](https://www.virtualbox.org)

* [Vagrant](http://vagrantup.com)

これらを事前にインストールしてください。

## Virtual Machineの構築方法

    host $ git clone https://github.com/iz-j/rails-dev-ja.git
    host $ cd rails-dev-ja
    host $ vagrant up

これだけです。

インストール完了後、次のコマンドでVMにアクセスできます。

    host $ vagrant ssh
    Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0-36-generic i686)
    ...
    vagrant@rails-dev-box:~$

VM上で実行したアプリには、ホストPCからは`localhost:3000`でアクセスできます。  
Webサーバーは127.0.0.1ではなく`0.0.0.0`にバインドしましょう。

    bin/rails server -b 0.0.0.0

## 同梱されるもの

* Development tools

* Git

* Ruby 2.2

* Bundler

* SQLite3, MySQL, and Postgres

* データベースとActive Record test suiteを実行する為のユーザー

* System dependencies for nokogiri, sqlite3, mysql, mysql2, and pg

* Memcached

* Redis

* RabbitMQ

* An ExecJS runtime

## 推奨ワークフロー

基本的な流れは、

* ホストPCでプログラムを編集して

* VM上でテストを実行

です。

まずは、あなたのRailsプロジェクトをこのプロジェクトのディレクトリにclone:

    host $ ls
    bootstrap.sh MIT-LICENSE README.md Vagrantfile
    host $ git clone git@github.com:<your username>/rails-app.git

cloneしたRailsのディレクトリは、VM上では`/vagrant`にマウントされます:

    vagrant@rails-dev-box:~$ ls /vagrant
    bootstrap.sh MIT-LICENSE rails-app README.md Vagrantfile

gemのインストールはそこで実行します:

    vagrant@rails-dev-box:~$ cd /vagrant/rails-app
    vagrant@rails-dev-box:/vagrant/rails-app$ bundle

これで、ホストPCでプログラムを編集して、VM上でテストする準備が整いました。

いつも使っているエディタや、Gitの設定、SSHキーの配置などをそのまま活かせるから、とても便利です。

## VMの管理

`^D`でログアウトして、

    host $ vagrant suspend

で、VMを停止します。  
再開するには、

    host $ vagrant resume

です。

    host $ vagrant halt

で、VMをシャットダウンし、

    host $ vagrant up

で、再起動します。

VMの状態を確認するには、次のコマンドを実行します。

    host $ vagrant status

ディスク上からVMのすべてのコンテンツを破棄するには、次のコマンドを実行します。

    host $ vagrant destroy # DANGER: all is gone

詳しい情報は [Vagrant documentation](http://docs.vagrantup.com/v2/) で確認してください。

## ファイル同期「rsync」

Vagrant 1.5からは、[sharing mechanism based on rsync](https://www.vagrantup.com/blog/feature-preview-vagrant-1-5-rsync.html)
が利用可能で、ファイルの読み書きが劇的に改善されています。  
`Vagrantfile`に以下の記述を追加するだけで、この機能が有効になります。

    config.vm.synced_folder '.', '/vagrant', type: 'rsync'

ファイルを監視させるには、

    host $ vagrant rsync-auto

です。

### rsync for Windows

rsyncをインストールする必要があります。  
Cygwinやmingw-get等を利用してインストールする方法もありますが、Gitがインストール済であれば、  
Git for Windowsに'rsync for win'ディレクトリ内ののdllとexeを配置する方法もあります。  
（例えば、`C:\Program Files (x86)\Git\bin`以下）

## Railsでサンプル起動まで

    host $ vagrant up
    host $ vagrant rsync-auto
    host $ vagrant ssh

    vagrant@rails-dev-box:~$ rails new sample
    vagrant@rails-dev-box:~$ cd sample
    vagrant@rails-dev-box:~$ vim Gemfile  # therubyracer をコメント解除
    vagrant@rails-dev-box:~$ bundle
    vagrant@rails-dev-box:~$ rails s -b 0.0.0.0

### VM→ホストPCのファイル同期

現時点では、`vagrant rsync-auto`によるファイル同期はホストPC→VMの一方向のみです。  

    vagrant plugin install vagrant-rsync-back

して、

    vagrant rsync-back

でVM→ホストPCの同期ができます。  
双方向同期が実装されるまでは、この手法で。

## オリジナルとの変更点

* Ubuntu32→Ubuntu64
* rsyncをデフォルトで有効に
* 各種データベースのポートを開放

## License

こちらへ。
https://github.com/rails/rails-dev-box
