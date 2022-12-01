---
icon: square-fill
order: 1
---
# Server

```js
const server = await Server({
  ip: '0.0.0.0',
  port: 27015,
  timeout: 3000,
});

const info = await server.getInfo();
console.log(info);

const players = await server.getPlayers();
console.log(players);

const rules = await server.getRules();
console.log(rules);

const ping = server.lastPing;
console.log(ping);

/*
You can also do:

const [info, players, rules] = await Promise.all([
  server.getInfo(),
  server.getPlayers().catch(e => {}),
  server.getRules().catch(e => {})
]);

This make all queries in parallel
*/
```

==- Options

These are the default values

```js
{
  ip: 'localhost',
  port: 27015,
  
  timeout: 2000,
  debug: false,
  enableWarns: true,
}
```
===

## getInfo()

Performs an [A2S_INFO](https://developer.valvesoftware.com/wiki/Server_queries#A2S_INFO) query to the server

```js
server.getInfo()
  .then(info => {
    console.log(info);
  })
  .catch(err => {
    //...
  });
```

Info can be something like this:

```js
{
  address: '192.223.30.25:27015',
  ping: 222,
  protocol: 17,
  goldSource: false,
  name: '[Raidboss] Private KZ/Climb [GOKZ |Global | VIP/Whitelist Only]',
  map: 'kz_frozen_go',
  folder: 'csgo',
  game: 'Counter-Strike: Global Offensive',
  appID: 730n,
  players: { online: 3, max: 10, bots: 0 },
  type: 'dedicated',
  OS: 'linux',
  visibility: 'public',
  VAC: true,
  version: '1.37.6.9',
  //data after this may not be present
  port: 27015,
  steamID: 85568392922144671n,
  tv: {
    port: 27020,
    name: 'RaidbossTV'
  },
  keywords: [
    'empty',     '5v5',
    'boss',    'casual',
    'climb',     'comp',
    'competitive', 'esea',
    'faceit',    'gloves',
    'knife',     'kreedz',
    'kz',      'ladder',
    'priz',    'pub',
    'pug',     'raid',
    'raidboss',  'rank',
    'ranks',     'st'
  ],
  gameID: 730n
}
```

## getPlayers()

Performs an [A2S_PLAYER](https://developer.valvesoftware.com/wiki/Server_queries#A2S_PLAYER) query to get the list of players in the server

Some servers may have disable this query (very rare).

```js
server.getPlayers()
  .then(players => {
    console.log(`There are ${players.length} in the server`);

    const list = players
      .sort((a, b) => b.timeOnline - a.timeOnline)
      .map((player, index) => `${index + 1}. ${player.name} ${player.timeOnline}`)
      .join('\n');

    console.log(list);
  })
  .catch(console.error);
```

> The `time` class has an personalized `toString()` and `@@toPrimitive()` methods

Example:

```js
[
  {
    index: 0,
    name: 'kritikal',
    score: 0,
    timeOnline: Time {
      hours: 0,
      minutes: 10,
      seconds: 25,
      start: 2021-03-20T02:46:28.267Z, //Date
      raw: 625.2186279296875
    }
  },
  {
    index: 0,
    name: 'dmx;',
    score: 0,
    timeOnline: Time {
      hours: 0,
      minutes: 10,
      seconds: 25,
      start: 2021-03-20T02:46:28.267Z,
      raw: 625.0791625976562
    }
  },
  {
    index: 0,
    name: 'fenakz',
    score: 0,
    timeOnline: Time {
      hours: 0,
      minutes: 10,
      seconds: 21,
      start: 2021-03-20T02:46:28.271Z,
      raw: 621.9488525390625
    }
  },
  {
    index: 0,
    name: '[JC] Master-cba',
    score: 0,
    timeOnline: Time {
      hours: 0,
      minutes: 10,
      seconds: 15,
      start: 2021-03-20T02:46:28.278Z,
      raw: 615.4395751953125
    }
  },
  {
    index: 0,
    name: 'sAIONARAH34! [pw] ⚓✵♣',
    score: 1,
    timeOnline: Time {
      hours: 0,
      minutes: 10,
      seconds: 1,
      start: 2021-03-20T02:46:28.292Z,
      raw: 601.7681274414062
    }
  },
  {
    index: 0,
    name: 'INFRA-',
    score: 0,
    timeOnline: Time {
      hours: 0,
      minutes: 9,
      seconds: 50,
      start: 2021-03-20T02:46:28.303Z,
      raw: 590.30859375
    }
  },
  {
    index: 0,
    name: 'Agente86',
    score: 1,
    timeOnline: Time {
      hours: 0,
      minutes: 9,
      seconds: 0,
      start: 2021-03-20T02:46:28.353Z,
      raw: 540.4190063476562
    }
  }
]
```

If the game is `The Ship` every player will have 2 extra properties `deaths` and `money`

## getRules()

Makes an [A2S_RULES](https://developer.valvesoftware.com/wiki/Server_queries#A2S_RULES) query to the server

Some servers may have disable this query, so you should expect an error.

```js
server.getRules()
  .then(console.log)
  .catch(() => {});
```

The response can change a lot between servers

Example:

```js
{
  bot_quota: 0,
  coop: 0,
  cssdm_enabled: 0,
  cssdm_ffa_enabled: 0,
  cssdm_version: '2.1.6-dev',
  deathmatch: 1,
  decalfrequency: 10,
  gungame_enabled: 1,
  metamod_version: '1.10.7-devV',
  mp_allowNPCs: 1,
  mp_autocrosshair: 1,
  mp_autoteambalance: 1,
  mp_c4timer: 35,
  mp_disable_respawn_times: 0,
  mp_fadetoblack: 0,
  mp_falldamage: 0,
  mp_flashlight: 1,
  mp_footsteps: 1,
  mp_forceautoteam: 0,
  mp_forcerespawn: 1,
  mp_fraglimit: 0,
  mp_freezetime: 1,
  mp_friendlyfire: 0,
  mp_holiday_nogifts: 0,
  mp_hostagepenalty: 13,
  mp_limitteams: 2,
  mp_match_end_at_timelimit: 0,
  mp_maxrounds: 0,
  mp_respawnwavetime: 10,
  mp_roundtime: 9,
  mp_scrambleteams_auto: 1,
  mp_scrambleteams_auto_windifference: 2,
  mp_stalemate_enable: 0,
  mp_stalemate_meleeonly: 0,
  mp_startmoney: 800,
  mp_teamlist: 'hgrunt;scientist',
  mp_teamplay: 0,
  mp_timelimit: 18,
  mp_tournament: 0,
  mp_weaponstay: 0,
  mp_winlimit: 0,
  nextlevel: '',
  r_AirboatViewDampenDamp: 1,
  r_AirboatViewDampenFreq: 7,
  r_AirboatViewZHeight: 0,
  r_JeepViewDampenDamp: 1,
  r_JeepViewDampenFreq: 7,
  r_JeepViewZHeight: 10,
  r_VehicleViewDampen: 1,
  scc_version: '2.0.0',
  sm_advertisements_version: 0.6,
  sm_allchat_version: '1.1.1',
  sm_cannounce_version: 1.8,
  sm_ggdm_version: '1.8.0',
  sm_gungamesm_version: '1.2.16.0',
  sm_nextmap: 'gg_toon_poolday',
  sm_noblock: 1,
  sm_playersvotes_version: '1.5.0',
  sm_quakesounds_version: 1.8,
  sm_resetscore_version: '2.6.0',
  sm_show_damage_version: '1.0.7',
  sm_vbping_version: 1.4,
  sourcemod_version: '1.10.0.6482',
  sv_accelerate: 5,
  sv_airaccelerate: 10,
  sv_allowminmodels: 1,
  sv_alltalk: 1,
  sv_bounce: 0,
  sv_cheats: 0,
  sv_competitive_minspec: 0,
  sv_contact: 'linkinaz0@vtr.net',
  sv_enableboost: 0,
  sv_enablebunnyhopping: 0,
  sv_footsteps: 1,
  sv_friction: 4,
  sv_gravity: 800,
  sv_maxspeed: 320,
  sv_maxusrcmdprocessticks: 24,
  sv_noclipaccelerate: 5,
  sv_noclipspeed: 5,
  sv_nostats: 0,
  sv_password: 0,
  sv_pausable: 0,
  sv_rollangle: 0,
  sv_rollspeed: 200,
  sv_specaccelerate: 5,
  sv_specnoclip: 1,
  sv_specspeed: 3,
  sv_steamgroup: '',
  sv_stepsize: 18,
  sv_stopspeed: 75,
  sv_tags: 'alltalk',
  sv_voiceenable: 1,
  sv_vote_quorum_ratio: 0.6,
  sv_wateraccelerate: 10,
  sv_waterfriction: 1,
  tf_arena_max_streak: 3,
  tf_arena_preround_time: 10,
  tf_arena_round_time: 0,
  tf_arena_use_queue: 1,
  tv_enable: 0,
  tv_password: 0,
  tv_relaypassword: 0
  }
```

## ping()

Performs an [A2A_PING](https://developer.valvesoftware.com/wiki/Server_queries#A2A_PING) query into the server



> This is a deprecated feature of source servers, may not work. The `server.lastPing` property contains the server ping, so this is not necessary

A warn in console will be shown (you can disable it by using `{ enableWarns: false }`, see [Options](#options))

It will return `-1` if the server did not respond to the query

```js
server.ping()
  .then(ping => {
    console.log(ping); // 214
  })
  .catch(console.error)
```

## disconnect()

Disconnect the server and destroy the socket

```js
server.disconnect();
```

### Use example

```js
Server(...)
  .then(async server => {
    const players = await server.getPlayers();

    server.disconnect();

    //...
  })
  .catch(console.error);
```

## static getInfo()

The difference is that it does not require the extra step of connection

Returns a promise that is resolved in an object with the server information, example:


```js
const { Server } = require('@fabricio-191/valve-server-query');

Server.getInfo({
  ip: '0.0.0.0',
  port: 27015,
})
  .then(console.log)
  .catch(console.error);
```