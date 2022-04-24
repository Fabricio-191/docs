---
icon: square-fill
---
# MasterServer

### Warns: 
* Gold source master servers may not work. The reason is unknown, the servers simply do not respond to queries
* If the quantity is `Infinity` or `'all'`, it will try to get all the servers, but most likely it will end up giving an warning (it will as many servers as possible). The master server stops responding at a certain point.

```js
MasterServer({
  quantity: 1000, // or Infinity or 'all'
  region: 'US_EAST',
  timeout: 3000,
})
  .then(servers => {
    //do something...

    //servers is an array if 'ip:port' strings, see below
  })
  .catch(console.error);
```

### Valid regions are
* US_EAST
* US_WEST
* SOUTH_AMERICA
* EUROPE
* ASIA
* AUSTRALIA
* MIDDLE_EAST
* AFRICA
* OTHER

==- Response example
```js
[
  '190.195.150.143:27015', '143.255.142.150:27015', '177.54.144.122:27523',
  '189.1.173.26:27015',  '177.144.128.13:27015',  '177.66.222.92:27015',
  '196.28.69.113:27065',   '144.48.37.119:27015',   '139.180.174.191:27051',
  '108.61.168.31:27050',   '108.61.168.31:27015',   '108.61.168.31:27051',
  '139.180.174.191:27052', '139.180.174.191:27053', '139.99.173.74:27015',
  '45.121.210.91:27550',   '139.99.131.105:27015',  '139.99.144.39:27015',
  '34.87.217.246:27015',   '108.61.227.50:27025',   '108.61.227.12:27035',
  '220.240.1.134:27023',   '221.121.159.236:35240', '221.121.159.236:35260',
  '221.121.159.236:35250', '221.121.149.12:32860',  '121.74.206.225:27015',
  '41.190.141.250:27015',  '111.221.44.137:27018',  '111.221.44.137:27024',
  '111.221.44.137:27023',  '49.245.116.134:27017',  '49.245.116.134:27025',
  '49.245.116.134:27027',  '49.245.116.134:27015',  '49.245.116.134:27016',
  '49.245.116.134:27026',  '223.25.71.43:27015',  '49.245.116.134:27115',
  '49.245.116.134:27118',  '49.245.116.134:27117',  '49.245.116.134:27116',
  '13.229.55.66:24000',  '49.245.116.134:27215',  '103.9.159.78:27065',
  '75.85.184.227:27031',   '75.85.184.227:27034',   '168.235.81.229:27015',
  '66.55.74.111:27015',  '66.55.74.100:27015',  '66.55.74.82:27015',  
  '66.55.74.38:27015',   '66.55.74.103:27015',  '66.55.68.38:27015',  
  '66.55.74.65:27015',   '66.55.74.105:27015',  '66.55.74.104:27015',
  '66.55.68.36:27015',   '66.55.70.53:27015',   '64.111.99.165:27020',
  '66.55.70.177:27115',  '66.55.70.177:27315',  '104.153.109.22:27015',
  '104.153.109.26:27015',  '47.153.235.28:27015',   '173.199.84.186:27015',
  '64.190.203.117:27015',  '103.214.108.12:27105',  '173.199.87.235:27025',
  '8.3.6.148:27015',     '66.75.2.253:27015',   '172.107.198.173:27075',
  '172.107.2.177:27035',   '198.12.71.30:27015',  '206.251.72.62:27017',
  '104.207.148.159:27045', '92.38.148.25:27015',  '159.89.142.219:27016',
  '159.89.142.219:27015',  '159.89.142.219:27017',  '74.91.118.231:27015',
  '108.61.124.77:27065',   '192.53.126.95:27015',   '108.61.124.78:27980',
  '108.61.235.138:27015',  '108.61.124.72:27045',   '104.206.244.2:19001',
  '198.24.171.83:27185',   '131.153.29.243:27035',  '173.27.92.73:27016',
  '162.248.90.33:27015',   '162.248.90.38:27015',   '162.248.90.19:27015',
  '66.58.130.164:27015',   '71.193.199.206:27015',  '71.193.199.206:27017',
  '54.202.134.208:27015',  '64.42.176.58:27015',  '104.192.227.146:17741',
  '74.201.72.18:27015',
  ...1051 more items
]
```
===

## Filter

```js
const filter = new MasterServer.Filter()
  .addFlag('dedicated')
  .add('map', 'cs_italy')
  .addNOR(
    new MasterServer.Filter()
      .addFlag('secure')
  );

MasterServer(
  quantity: 1000,
  region: 'EUROPE',
  timeout: 3000,
  filter,
})
  .then(console.log)
  .catch(console.error)

// Will return a list of ips that are dedicated servers, in the cs_italy map and that do not have an anti-cheat enabled
```

`add(key, value)` adds a condition to the filter.  
`addFlag(flag)` adds a flag to the filter.  
`addFlags([flag, flag, ...])` adds multiple flags to the filter.  
`addNOR(filter)` a special condition, specifies that servers matching any of the `filter` conditions should not be returned.  
`addNAND(filter)` a special condition, specifies that servers matching all of the `filter` conditions should not be returned.  

See https://developer.valvesoftware.com/wiki/Master_Server_Query_Protocol#Filter

### Conditions

| Parameter          | Values  | Description                                                                   |
| ------------------ | ------- | ----------------------------------------------------------------------------- |
| name_match         | string  | Servers with their hostname matching that hostname (can use * as a wildcard)  |
| version_match      | string  | Servers running version that version (can use * as a wildcard)                |
| gameaddr           | string  | Return only servers on the specified IP address (port supported and optional) |
| gamedir            | string  | Servers running the specified modification (ex. cstrike)                      |
| map                | string  | Servers running the specified map (ex. cs_italy)                              |
| appid              | number  | Servers that are running game with that [appid](https://developer.valvesoftware.com/wiki/Steam_Application_IDs) |
| napp               | number  | Servers that are NOT running game with that [appid](https://developer.valvesoftware.com/wiki/Steam_Application_IDs) |
| gametype           | array   | Servers with all of the given tag(s) in sv_tags                               |
| gamedata           | array   | Servers with all of the given tag(s) in their 'hidden' tags (only in L4D2)    |
| gamedataor         | array   | Servers with any of the given tag(s) in their 'hidden' tags (only in L4D2)    |

Notes: 
* all array's are of strings.
* `flags` are not booleans. if you want to get the inverse of any of these flags, you should use `addNOR` or `addNAND`.

### Flags

| Name               | Description                                                               |
| ------------------ | ------------------------------------------------------------------------- |
| dedicated          | Servers that are dedicated servers                                        |
| secure             | Servers using anti-cheat technology (VAC, but potentially others as well) |
| linux              | Servers running on a Linux platform                                       |
| password           | Servers that are not password protected                                   |
| noplayers          | Servers that are empty                                                    |
| empty              | Servers that are not empty                                                |
| full               | Servers that are not full                                                 |
| proxy              | Servers that are spectator proxies                                        |
| white              | Servers that are whitelisted                                              |
| collapse_addr_hash | Return only one server for each unique IP address matched                 |

## Master servers

See https://developer.valvesoftware.com/wiki/Master_Server_Query_Protocol#Master_servers

### Source

The port used in `hl2master.steampowered.com` ip's is `27011` but one of them is using a different port: `27015`  

### GoldSource

The port numbers used by `hl1master.steampowered.com` can be anything between `27010` and `27013`.