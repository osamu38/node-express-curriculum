# Node.jsを使ってアプリケーションを構築しよう！

## 目次

  1. Node.jsを導入しよう
  1. Expressを導入しよう
  1. Socket.IOを使って双方向通信を楽しんでみよう

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


