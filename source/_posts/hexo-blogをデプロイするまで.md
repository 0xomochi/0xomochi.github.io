---
title: hexo blogをデプロイするまで
date: 2018-09-07 10:49:46
categories:
- hexo
tags: 
- hexo
- blog
---
## モチベーション
・[静的サイトジェネレーター](https://qiita.com/okmttdhr/items/82ecb0332835472e905f#hexojavascriptgithub-star-4204)でブログを作りたい  
・ブログ構築しつつGitHubで差分管理しておきたい -> [GitHub Pages](https://pages.github.com/)  

## 環境
MacOS High Sierra 10.13.6  
homebrewインストール済み([homebrewのインストールについて](https://brew.sh/index_ja))  

## 手順
かなり雑に書いているので参考サイト参照  
### Github Pages用のリポジトリを作る
自分のGitHubにアクセスし、新しいリポジトリを作成  
リポジトリ名は"<replace_username>.github.io"にする  
ブログのurlが"https://<replace_username>.github.io"になる  

### Homebrewでnodebrewをインストール  
```
$ brew install node brew
```
.bash_profileに追記して.bash_profileを再読み込み  
pathの指定でミスってvimなどの主要コマンドが使えなくなった場合  
-> [対処](https://yunabe.hatenablog.com/entry/2017/02/11/134355)  
```
$ source ~/.bash_profile
```
node.js(最新版)をインストールする  
```
$ nodebrew install-binary latest
Fetching: https://nodejs.org/dist/v10.10.0/node-v10.10.0-darwin-x64.tar.gz
######################################################################## 100.0%
Installed successfully

$ nodebrew list
v10.10.0

current: none

$ nodebrew use v10.10.0
$ nodebrew list
v10.10.0

current: v10.10.0

$ npm -v
6.2.0
```
"$ nodebrew install-binary latest"がうまくいかなかった時は  
-> [対処法](https://qiita.com/yn01/items/d1fa10dbe4850f7cd693)
これでもうまくいかない場合は上記コマンドにsudoをつけて実行  

### hexoとデプロイツールを導入  
```
$ npm install hexo
$ npm install hexo-deployer-git --save
```

### デプロイ
```
$ mkdir myblog
$ cd myblog
$ hexo init
$ hexo server
```
これでlocalhost:4000でデプロイしたページが見れる  
ctr+Cで終了  

### _config.ymlの修正
themeによって変化するので割愛[参考](https://qiita.com/wawawa/items/1a2f174fb29c35302543)  

### 好みのThemeを探す
・[hexo themes 公式](https://hexo.io/themes/)  
・[hexo themes wiki](https://github.com/hexojs/hexo/wiki/Themes)  
・本ブログで使っているtheme -> [sabrinaluo/hexo-theme-replica](https://github.com/sabrinaluo/hexo-theme-replica)  

### デプロイする
```
$ git init
$ git remote add origin git@github.com:<replace_username>/<replace_username>.github.io.git
$ hexo deploy -g
```
これで"https://<replace_username>.github.io"にアクセスしてブログが見れればdone  
新しい記事を作る手順は本ブログの[Hello World](https://0xomochi.github.io/2018/09/07/hello-world/)にある  

### postの削除方法
```
$ rm <your_dir>/source/_posts/<your_post.md>
$ hexo clean
$ hexo generate
```
### categoriesとtagsの404解消
```
$ hexo new page categories
$ hexo new page tags
```

## 参考にしたサイト
[Hexoで始めるお手軽な静的ブログ　-インストールと配備-](https://qiita.com/in_silico_/items/7e6ed639c24142bdbd04)  
[HexoブログをGitHub Pagesで最速公開する](https://tech.qookie.jp/posts/hexo-deploy-github-pages-fast/)  
[Hexoでgithubにデプロイする](https://qiita.com/f_prg/items/d10a77b1e356a46d9ab9)  
[【Hexo入門】Hexoでブログを作成する時のTipsまとめ](https://qiita.com/jhChoi/items/85f3b5ba39619bc47f4d)  
[Troubleshooting](https://hexo.io/docs/troubleshooting.html)  
[Hexoを使って個人ブログ作成, Github Pagesにデプロイするまで](https://qiita.com/wawawa/items/1a2f174fb29c35302543)  
[HexoのFront-matterにカテゴリーとタグを上手く設定する方法](https://tech.qookie.jp/posts/hexo-frontmatter-category-tag/)  
[nodebrew ls でエラーになります。](https://teratail.com/questions/24309)  
[nodebrewでよく使うコマンド](https://qiita.com/suisuina/items/c5c4e4b9f55a8615a542)  
[おれろぐ #z_a_ki3/node.jsの環境構築（mac）](http://zaki3.hateblo.jp/entry/2018/03/21/111738)  
[小白妹妹写代码](http://sabrinaluo.github.io/tech/)  

