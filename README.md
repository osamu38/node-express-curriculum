
Express導入してみよう

$ cd 任意のディレクトリ
$ mkdir node-test
$ cd node-test
$ npm install -g express-generator
$ express -e
$ cd . && npm install
$ DEBUG=node:* ./bin/www

localhost:3000にアクセスしてExpressが表示されていればおkです。

Socket.IOを使って双方向通信を楽しんでみよう

$ npm install socket.io --save

wwwの30行目（server.on('listening', onListening);）の下に

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

をコピペ

index.ejsのbodyの閉じタグの上に

<script src="/socket.io/socket.io.js"></script>
<script>
var socket = io.connect();
socket.emit('request');
socket.on('response', function() {
    alert('Get Response!!');
});
</script>

をコピペ
