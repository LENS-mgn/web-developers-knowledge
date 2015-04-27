既存サイトが既に運用されている場合。WordMove 利用を前提、使えない場合に rsync や scp を併用する。
[rsyncを使った熟練者レベルのバックアップ - IT media](http://www.itmedia.co.jp/enterprise/articles/0707/19/news059.html)

## テスト/本番サイトバックアップ
- downloads
- plugins
- DBdump

```
$ scp -rp 10022 user@ssh.com:/path/to/wp-content/plugins ~/Downloads/sitename
$ scp -rp 10022 user@ssh.com:/path/to/wp-content/uplaods ~/Downlods/sitename
```

でローカルにコピー、3点セットを zip にまとめ、オンラインストレージにて共有。



## 実環境やステージングの変更をローカル環境にマージ
WordMove、rsync や scp と git を併用しマージを行う。



## 本番サイトを退避し、ローカル環境をサーバにアップロード

- 本番環境のディレクトリをサーバ上でコピー
- コピーしたディレクトリに本番環境名_bk などリネームしておく

```
$ mv path/to/site_dir path/to/site_dir.bak
```

- wordmove や rsync で path/to/site_dir にローカルのファイルをアップロード


## DB移動 テスト → 本番
この時点で一度サイトが見られなくなる

- `wordmove push -d -e production`
- sequel
- navicat


## DBドメイン置換
- WordMove を利用した場合には不要
- https://interconnectit.com/products/search-and-replace-for-wordpress-databases/
- ドメインを置換
- `$ git clone https://github.com/interconnectit/Search-Replace-DB.git`
- http://hogehoge.com/Search-Replace-DB
- `$ rm -rf Search-Replace-DB.git/`


## サイト確認

- 大抵 DB 登録されている何かがまちがっているから調査と修正
    - ウィジェット
    - カスタムメニュー
- httpsになる場合
    - mw wp form で確認画面とかの登録がhttpのままだとリダイレクトループに陥る
- サムネイル画像のサイズを別途作った場合は再生成必須 [Regenerate Thumbnails](https://wordpress.org/plugins/regenerate-thumbnails/)
- 検索エンジンの対応 stop the ボケッチ



# 明らかに大失敗の場合

- DBファイルを戻す
- 新本番環境と急本番環境の名前を差し替える

```
$ mv site_dir.bak site_dir
```



# rsync のコマンドスニペット

サーバ上の uploads をローカルに同期

```
$ rsync -r ssh-user@ssh-host:/var/www/wp-content/uploads/ ..../wp-content/uploads
```

ローカルのテーマをサーバに同期

```
$ rsync -r --exclude=node_modules/ themename/ ssh-user@ssh-hostname:/remote/themename/
```

ポート番号を指定する場合

```
$ rsync -e 'ssh -p 10022'
```

```
$ rsync -r temp01/ temp02
```
実行時、temp01 と temp02 内に同じファイルがあった際、temp01 の内容が優先される
