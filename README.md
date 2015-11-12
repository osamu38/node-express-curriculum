# Node.jsを使ってアプリケーションを構築しよう！

## 目次

  1. Node.jsとは？
  1. Node.jsを導入しよう
  1. Expressを導入しよう
  1. Socket.IOを使って双方向通信を楽しんでみよう

## Node.jsとは？

いきなり難しいことを書いてもよくわからないと思うので、簡単に説明しますと、JavaScriptと同じ記法で書けるサーバーサイド言語です。

#### Node.jsの利点

+ 他の言語と比べ、双方向通信が得意
+ 既にJavaScriptを勉強している人だと勉強コストが低い
+ クライアントとソースを共有できる
+ 導入コストが低い
+ コンパイルする必要がない
+ 割と簡単
+ **超面白い**

#### Node.jsの欠点

+ 非同期処理がやたら多い（コールバック地獄問題）
+ 技術者が少ない
+ ドキュメントが少ない
+ Node.js単体の案件が少ない

#### Node.jsを使う上で必要な単語の説明

<dl>
  <dt>nvm</dt>
  <dd>Node Version Managerの略。Node.jsのバージョンは頻繁に更新されるため、アップデートがかかるたびに公式サイトから新しいNode.jsを落としてくるのは面倒なので、nvmを使ってバージョンを管理する。非常に便利。</dd>
  <dt>npm</dt>
  <dd>Node Package Managerの略。後述するnode_modulesを管理するためのコマンド。Node.jsを導入することで使用できる。<code>npm install</code>を叩くことでpackage.jsonに記述されているnode_modulesをinstallする。</dd>
  <dt>node_modules</dt>
  <dd>Node.jsの開発に必要なモジュールのこと。</dd>
  <dt>package.json</dt>
  <dd>Node.jsの開発に必要な情報が記述された設定ファイル。</dd>
  <dt>.bash_profile</dt>
  <dd>.bash_profileはTerminalを起動時に.bash_profileに記述されているShellScriptを実行してくれる。nvmはターミナル起動時だとpathが通っていないので使えないが、.bash_profileに記述するばいける。便利。</dd>
  <dt>Express</dt>
  <dd>Node.js Web Application Framework。大した機能はないが、導入が非常に簡単でわかりやすい。おそらくNode.jsで一番使われているFramework。</dd>
  <dt>Socket.IO</dt>
  <dd>双方向通信を実装するためのnode_modules。他の言語に比べてとても簡単に導入出来るため、この機能目当てにNode.jsを採用するプロジェクトも多い。</dd>
</dl>

## Node.jsを導入しよう

```
$ brew install nvm (nvmのinstall)
$ vi .bash_profile (.bash_profileファイルを作成し、viで編集)

# nvm設定
[[ -s ~/.nvm/nvm.sh ]] && . ~/.nvm/nvm.sh
nvm use default
npm_dir=${NVM_PATH}_modules
export NODE_PATH=$npm_dir

↑を.bash_profileにコピペ

$ source ~/.bash_profile (.bash_profileを適用)
$ nvm ls-remote (Node.jsのバージョンを一覧で表示)

　v0.1.14
　v0.1.15
　:
　:
　v4.2.2
　v5.0.0

$ nvm install v5.0.0 (v5.0.0のNode.jsをinstall)
$ nvm alias default v5.0.0 (defaultで使用するNode.jsを設定)
$ nvm use v5.0.0 (v5.0.0のNode.jsを使用)
```

## Expressを導入しよう

```
$ cd 任意のディレクトリ (任意のディレクトリに移動)
$ mkdir node-test (任意のディレクトリにnode-testというフォルダを作成)
$ cd node-test (node-testに移動)
$ npm install -g express-generator (グローバルでexpress-generatorを導入)
$ express -e (expressのゴーストアプリを作成)
$ cd . && npm install (必要なnode_modulesをinstall)
$ DEBUG=node:* ./bin/www (アプリを立ち上げ)
```

localhost:3000にアクセスしてExpressが表示されていればおkです。

## Socket.IOを使って双方向通信を楽しんでみよう

#### Socket.IOを導入しよう

```
$ npm install socket.io --save (socket.ioをinstall)
```

#### クライアントからリクエストを送ろう

/views/index.ejsの9行目（`<p>Welcome to <%= title %></p>`）の下に以下のソースをコピペ

```JavaScript
<button>Request!!</button>
<script src="https://code.jquery.com/jquery-2.1.3.min.js"></script>
<script src="/socket.io/socket.io.js"></script>
<script>
var socket = io.connect();
$('button').on('click', function() {
    socket.emit('request');
});
socket.on('response', function() {
    alert('Get Response!!');
});
</script>
```

#### サーバーでリクエストを受け取ってレスポンスを返そう

/bin/wwwの30行目（`server.on('listening', onListening);`）の下に以下のソースをコピペ

```JavaScript
var socketIO = require('socket.io');
var io = socketIO.listen(server);

io.on('connection', function(socket) {
    socket.on('disconnect', function() {
        console.log('user disconnected.');
    });
    socket.on('request', function() {
        socket.broadcast.emit('response');
    });
});
```


