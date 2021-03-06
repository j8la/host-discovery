host-discovery
=============
A simple service which sends messages to a multicast address in JSON format. It is designed to run on multiple hosts to maintain a list of subscribers to the multicast group. That can be useful for creating a distributed service. A filter, represented by the "service" field, can be used to filter messages if other hosts emit packets on the same multicast address. IPv4 and IPv6 are supported.

## Installation
```
npm install host-discovery
```

## How to

### Import module
```javascript
var hostDiscovery = require('host-discovery');
```

### Create instance
```javascript
var service = new hostDiscovery({
    service: 'test',    // Default to 'all'
    protocol: 'udp4',   // Default to udp4, can be udp6
    port: 60600         // Default to 2900
});
```

If default options are sufficient for you, you can create an instance like this :
```javascript
var service = new hostDiscovery();
```

Or for one option :
```javascript
var service = new hostDiscovery({
    protocol: 'udp6'    
});
```

### Events
Two events are implemented :
- `join` for new members of multicast group and service field
- `leave` for members which has left multicast group

```javascript
service.on('join', (ip) => { 
    console.log('A new member has joined the group : ' + ip); 
});

service.on('leave', (ip) => { 
    console.log('A member has left the group : ' + ip); 
});
```

The detection of a leaving host is between 10 and 15 seconds.

### Methods
To start the service :
```javascript
service.start();
```

To stop the service :
```javascript
service.stop();
```

To get active hosts :
```javascript
service.hosts();

// The result will be :
[ '192.168.0.11': { 
    address: '192.168.0.11',
    hostname: 'test01',
    timestamp: 1463782466523
  },
  '192.168.0.12': { 
    address: '192.168.0.12',
    hostname: 'test02',
    timestamp: 1463782466527
  },
  '192.168.0.13': { 
    address: '192.168.0.13',
    hostname: 'test03',
    timestamp: 1463782466537
  } 
]
```

## Updates
- `v1.0.5 :` Sometimes, we are doing stupid things. Timestamp field is back and host's desinscription magically works again.
- `v1.0.4 :` Remove "timestamp" field 
- `v1.0.3 :` Add "address" field 
- `v1.0.2 :` Fix "ReferenceError: os is not defined"
- `v1.0.1 :` Fix "ReferenceError: util is not defined"
- `v1.0.0 :` Initial release

## Licence
The MIT License (MIT) 
Copyright (c) 2016 Julien Blanc