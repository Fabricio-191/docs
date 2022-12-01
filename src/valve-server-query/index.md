---
icon: static/Orange_lambda.svg
expanded: false
hidden: false
---
## An implementation of valve protocols

```js
const { Server, RCON, MasterServer } = require('@fabricio-191/valve-server-query');
```

This module allows you to:

* Easily make queries to servers running valve games
* Make queries to valve master servers
* Use a remote console to control your server remotely

!!!warning

The valve protocol implementation varies between engines/games/servers

Warning:

* Some servers may have some queries disabled
* Some servers may have modified by hand the response to some queries
* Some servers queries response just don't follow the valve protocol

!!!


==- Known valve issues

Server

* Some servers may return the players list with offline players
* Some servers may return the players list cut in some point
* If there are more than 255 players in a server the `playersQty` byte in the players response is not enought (solved)

Master server

* If you give a filter to the master server in the returned list there may be some servers that don't follow the filter

RCON

* a

===
