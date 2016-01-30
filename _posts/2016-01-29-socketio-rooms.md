---
title: Socket.io and Rooms
updated: 2016-01-29
---

Using socket.io on the server side, we have the ability to emit an event to a particular socket, even outside of an event-emitter from that socket, if we know the id of the socket we are emitting to. Suppose the socket id is ```'7s6df58sg9as567d'```, one might logically conclude that this syntax would work:

```javascript
var io = require('socket.io').listen(server);
io.to('7s6df58sg9as567d').emit('MESSAGE');
```

There is a caveat here, plugging this string into ```some_socket_id``` will throw an error.

For individual sockets, it is necessary to add /# in front of the socket id:

```javascript
io.to('/#7s6df58sg9as567d').emit('MESSAGE');
```

Alternatively, if we wanted to emit a message to a room, the /# is not needed, and would take the following form:

```javascript
io.to('ROOM_NAME').emit('MESSAGE');
```

Now you know, not to spend far too much time trying to debug this error should you run into it.