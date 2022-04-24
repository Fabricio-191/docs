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

**Some** of the games where it works with are:

* Counter-Strike
* Counter-Strike: Global Offensive
* Garry's Mod
* Half-Life
* Team Fortress 2
* Day of Defeat
* The ship

<details>
<summary>Options</summary>
</br>

These are the default values

```js
{
  ip: 'localhost', //in MasterServer is 'hl2master.steampowered.com'
  port: 27015, //in MasterServer is 27011
  
  timeout: 2000,
  debug: false,
  enableWarns: true,

  //RCON
  password: 'The RCON password', // hasn't a default value
  
  //Master server
  quantity: 200,
  region: 'OTHER',
}
```
</br>
</details>
</br>
