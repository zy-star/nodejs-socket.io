# nodejs-socket.io
熟悉socket，练习demo，聊天室

###app.js
``` javascript
  var app = express();
  var http = require('http').Server(app);
  var io = require('socket.io')(http);
  ...
  ...
  io.on('connection', function(socket){
    socket.on('login', function(obj){
        socket.name = obj.userid;
          if(!onlineUsers.hasOwnProperty(obj.userid)) {
              onlineUsers[obj.userid] = obj.username;
              onlineCount++;
          }
         
        io.emit('login', {onlineUsers:onlineUsers, onlineCount:onlineCount, user:obj});
        console.log(obj.username+'加入了聊天室');
    });
     
    socket.on('disconnect', function(){
        if(onlineUsers.hasOwnProperty(socket.name)) {
            var obj = {userid:socket.name, username:onlineUsers[socket.name]};
             
            delete onlineUsers[socket.name];
            onlineCount--;
            io.emit('logout', {onlineUsers:onlineUsers, onlineCount:onlineCount, user:obj});
            console.log(obj.username+'退出了聊天室');
        }
    });
     
    socket.on('message', function(obj){
        io.emit('message', obj);
        console.log(obj.username+'说：'+obj.content);
    })
```
   
});
