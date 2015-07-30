## Node.jsを導入しよう

おそらく入ってる筈なので割愛

## Expressを導入しよう

```
$ cd 任意のディレクトリ
$ mkdir node-test
$ cd node-test
$ npm install -g express-generator
$ express -e
$ cd . && npm install
$ DEBUG=node:* ./bin/www
```

localhost:3000にアクセスしてExpressが表示されていればおkです。

## Socket.IOを使って双方向通信を楽しんでみよう

#### Socket.IOを導入

```
$ npm install socket.io --save
```

#### クライアントからリクエストを送ろう

/views/index.ejsの9行目（`<p>Welcome to <%= title %></p>`）の下に以下のソースをコピペ

```JavaScript
<script src="/socket.io/socket.io.js"></script>
<script>
var socket = io.connect();
socket.emit('request');
socket.on('response', function() {
    alert('Get Response!!');
});
</script>
```

#### サーバーでリクエストを受け取ってをレスポンスを返そう

/bin/wwwの30行目（`server.on('listening', onListening);`）の下に以下のソースをコピペ

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


