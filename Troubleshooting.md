### This is a place holder for the troubleshooting document to come.

# Troubleshooting issues with Lisk

The purpose of this document is to provide clarification around various issues that may be encountered during the installation and normal execution of Lisk.

##Table of Contents

#### Installation Issues

##### Prerequisite Checks Failed
##### Postgres Database Initialization Failed
##### Lisk Failed to Start

#### Runtime Issues

##### error: Forever detected script was killed by signal: SIGKILL


#### Fork Issues

##### Cause 1
##### Cause 2
##### Cause 3
##### Cause 5


## error: Forever detected script was killed by signal: SIGKILL

```
error: Forever detected script was killed by signal: SIGKILL
module.js:489
    throw err;
          ^
SyntaxError: /home/lisk/lisk-main/config.json: Unexpected token #
    at Object.parse (native)
    at Object.Module._extensions..json (module.js:486:27)
    at Module.load (module.js:355:32)
    at Function.Module._load (module.js:310:12)
    at Module.require (module.js:365:17)
    at require (module.js:384:17)
    at c (/home/lisk/lisk-main/app.js:1:216)
    at b (/home/lisk/lisk-main/app.js:1:1107)
    at Object.<anonymous> (/home/lisk/lisk-main/app.js:10:22075)
    at c (/home/lisk/lisk-main/app.js:1:477)
error: Forever detected script exited with code: 1
```
                                                     
