# XenaJS - WIP [![Build Status](https://travis-ci.com/gdyr/xenajs.svg?branch=master)](https://travis-ci.com/gdyr/xenajs)
_A NodeJS client library for Xena Networks' XenaScripting API_

XenaJS (NPM: xenascripting) provides a simple, promise-based API for controlling and monitoring Xena Networks devices using their XenaScripting API.

### Usage

```node
const Xena = require('xenascripting');

const xc1 = new Xena('10.0.0.151');
const xc2 = new Xena('10.0.0.152', {
  user: 'Michael',      // Display name for your session (default: "XenaScripting")
  password: 'password', // Logon password for chassis
  autoreserve: true,    // Automatically reserves all resources (default: false)
  autorelinquish: true, // Automatically relinquish resources reserved by others (default: false),
  refresh: 60,          // Seconds to cache chassis info for (default: 60 seconds) 
  debug: console        // Object to log debug messages to (default: none)
});

xc2.setTimeout(60);     // Connection timeout in seconds

xc2.connect()
  .then((chassis) => {

    // Chassis info
    console.log(chassis.name);          // Chassis name                             (String)
    console.log(chassis.description);   // Chassis description                      (String)
    console.log(chassis.password);      // Chassis password                         (String)
    console.log(chassis.model);         // Chassis model                            (String)
    console.log(chassis.serial);        // Chassis serial                           (String)
    console.log(chassis.version);       // Chassis firmware version                 (String)
    console.log(chassis.driver);        // Chassis PCI driver version               (String)
    console.log(chassis.capabilities);  // Chassis capabilities                     (Array)
    console.log()

    // Reservation
    console.log(chassis.mine);          // Chassis reserved by you                  (Boolean)
    console.log(chassis.reservation);   // Username of reservation, or false.       (String | false)
    chassis.relinquish();               // Kicks the previous user off the chassis. (Promise)
    chassis.reserve();                  // Reserves chassis for this session.       (Promise)

    // Sessions
    chassis.sessions.map((session) => {

      console.log(session.index);       // Session ID                               (Number)
      console.log(session.type);        // "Manager" or "Script"                    (String)
      console.log(session.address);     // Client IP address                        (String)
      console.log(session.user);        // Session display name                     (String)
      console.log(session.ops);         // Count of session operations              (Number)
      console.log(session.rx);          // Session bytes to chassis                 (Number)
      console.log(session.tx);          // Session bytes from chassis               (Number)
    
    });

    /* WIP */

    // Modules
    chassis.modules.map((module) => {



      module.ports.map((port) => {

        console.log(port.errors); // from C_PORTERRORS if need be

      })

    })

  });

```