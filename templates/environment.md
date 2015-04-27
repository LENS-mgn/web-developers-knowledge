スタティックサイトや WordPress 案件を想定し、[vccw](https://github.com/vccw-team/vccw) 及び [gulp-web-starter](https://github.com/vwxyutarooo/gulp-web-starter) を利用します。

# Requires
* sass < 3.4
* npm
* bower
* sass-globbing
* git
* vagrant

# 利用方法
## イニシャライズ
プロジェクトにおいて環境構築をする際、始めに一度だけ実行すること。
foundation / bootstrap, _s の自動インストールなどを行います。

```
$ git clone https://github.com/vwxyutarooo/gulp-web-starter.git themename
$ cd themename
$ rm -rf .git; rm -rf .gitignore
$ npm install; npm run gulp -- init; bower install
```

## プロジェクトメンバー
イニシャライズ実行後、git リポジトリにて共有された環境を clone 或いは pull した後

```
$ npm install
```


# Foundation について
* `src/scss/core/_foundation.scss` にて必要な機能のみを読み込む
* `src/scss/core/_settings.scss` にて foundation の変数をオーバーライド

## 関連リンク
- [リポジトリ](https://github.com/zurb/foundation)
- [ドキュメント](http://foundation.zurb.com/)



# gulp について
gulp のグローバルインストールを避けて実行する場合

    $ npm run gulp

特定のタスクを実行したい場合

    $ npm run gulp -- taskname

通常 browserSync はプロキシモードで動作し、Vagrant マシンに繋がります。プロトタイプ製作時には

    $ npm run gulp-server

を利用することで Jade から生成した html 参照用のサーバが立ち上がります。

## 運用するタスク
* bower_components 内の一部フィアル抽出
* `src/html` 用のスタティックサーバ起動 (`proxy = false` 時)
* `wp-usd.dev` 用の Livereload サーバ起動<br>(`proxy != false` 時)
* Jade のコンパイル<br>(`src/jade/*.jade` → `src/html/*.html`)
* sass のコンパイル<br>(`src/scss` → `assets/css/app.css`)
* Browserify<br>(`src/js/app/app.js` → `assets/js/bundle.js`)
* 画像圧縮<br>(`assets/imgaes/page/**/*.png` → `assets/images/page/**/*.png`)
* 画像スプライト生成<br>(`src/iamges/sprite/*.png` → `assets/sprite.png`)

## 関連リンク
- [リポジトリ](https://github.com/gulpjs/gulp)
- [ドキュメント](https://github.com/gulpjs/gulp/blob/master/docs/README.md)


## 関連リンク
* [公式サイト](http://underscores.me/)
* [リポジトリ](https://github.com/Automattic/_s)
* [詳しい解説サイト](http://gatespace.jp/category/wordpress/use_underscores/)



# MySQLの置換
- [DATABASE SEARCH AND REPLACE SCRIPT IN PHP](https://interconnectit.com/products/search-and-replace-for-wordpress-databases/)

設置

```
$ git clone https://github.com/interconnectit/Search-Replace-DB.git searchreplacedb
$ rm -rf searchreplacedb/.git
```

ベーシック認証の解除後、必ず削除すること

```
$rm -rf searchreplacedb/
```


# Gitを利用した運用について
- [サルでもわかるGit入門 〜バージョン管理を使いこなそう〜 | どこでもプロジェクト管理バックログ](http://www.backlog.jp/git-guide/)

## GitHub Flowについて
- 参考: [GitHub Flow日本語訳](https://gist.github.com/juno/3112343)
- 開発中はmasterでどんどん進めます。
    -  多少の conflict はなんとかなるので。
- 運用フェーズではGitHub Flowを参考に進めます。

## オレオレGitHub Flowについて
- 修正や機能追加などについて、その課題番号のIDでmasterからブランチを切り作業を進める
- 作業が完了したらmasterにマージ
- シンプルですね



# 開発環境の構築手順
[README.md](https://megane9988.backlog.jp/git/PROWREST/wp-usd/tree/master#loom) を参照
