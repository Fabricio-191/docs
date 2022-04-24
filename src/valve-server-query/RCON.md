---
icon: square-fill
---
# RCON

Some commands will cause the server to disconnect, for example `sv_gravity 0` in some cases or `rcon_password newPassword` as it changes de password and needs to authenticate again.

[Commands list](https://developer.valvesoftware.com/wiki/Console_Command_List) 

```js
const rcon = await RCON({
  ip: '0.0.0.0',
  port: 27015, //RCON port
  password: 'your RCON password'
});

rcon.on('disconnect', async (reason) => {
  console.log('disconnected', reason);
  try{
    await rcon.reconnect();
  }catch(e){
    console.log('reconnect failed', e.message);
  }
});

rcon.on('passwordChanged', async () => {
  const password = await getNewPasswordSomehow();
  try{
    await rcon.authenticate(password);
  }catch(e){
    console.error('Failed to authenticate with new password', e.message);
  }
});

const response = await rcon.exec('sv_gravity 1000');
// Response is always a string that is some kind of log of the server or it can be empty
```

<details>
<summary><code>exec(command)</code></summary>

This will work well with `server.getRules()`
```js
// gravity will change randomly every 5 seconds

setInterval(() => {
  const value = Math.floor(Math.random() * 10000) - 3000;
  //value will be a number between -3000 and 6999

  rcon.exec(`sv_gravity ${value}`)
    .catch(console.error);

}, 5000);
```
</details>


<details>
<summary><code>destroy()</code></summary>

Destroys the RCON connection, this will not fire the `disconnect` event

```js
rcon.destroy();
```
</details>
