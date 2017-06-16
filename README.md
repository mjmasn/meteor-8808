# meteor-8808
Reproduction of Meteor Issue #8808

**Instructions**

1. Clone this repo

   `git clone git@github.com:mjmasn/meteor-8808.git`

2. Open the directory

   `cd meteor-8808`
   
3. Build the app

   `meteor build ../meteor-8808-build`
   
4. Unpack the build

   `cd ../meteor-8808-build && tar -xf meteor-8808.tar.gz`
  
5. Open the server directory

   `cd bundle/programs/server`

6. Check node/npm versions

   `node -v && npm -v`
   
   (should be `v8.1.2` and `4.6.1`)
   
6. Run npm install

    `npm i`
    
7. Get failure

```
> fibers@2.0.0 install /Users/mike/Code/meteor-8808-build/bundle/programs/server/node_modules/fibers
> node build.js || nodejs build.js

`darwin-x64-57` exists; testing
Binary is fine; exiting

> meteor-dev-bundle@0.0.0 install /Users/mike/Code/meteor-8808-build/bundle/programs/server
> node npm-rebuild.js

events.js:182
      throw er; // Unhandled 'error' event
      ^

Error: spawn npm ENOENT
    at exports._errnoException (util.js:1016:11)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:189:19)
    at onErrorNT (internal/child_process.js:366:16)
    at _combinedTickCallback (internal/process/next_tick.js:102:11)
    at process._tickCallback (internal/process/next_tick.js:161:9)
    at Function.Module.runMain (module.js:607:11)
    at startup (bootstrap_node.js:158:16)
    at bootstrap_node.js:575:3
npm WARN meteor-dev-bundle@0.0.0 No description
npm WARN meteor-dev-bundle@0.0.0 No repository field.
npm WARN meteor-dev-bundle@0.0.0 No license field.
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! meteor-dev-bundle@0.0.0 install: `node npm-rebuild.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the meteor-dev-bundle@0.0.0 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/mike/.npm/_logs/2017-06-16T08_49_42_949Z-debug.log
```

**Extra credit:**

Edit npm-rebuild.js from:
```js
// Make sure the npm finds this exact version of node in its $PATH.
var binDir = path.dirname(process.execPath);
var PATH = binDir + path.delimiter + process.env.PATH;
var env = Object.create(process.env, {
  PATH: { value: PATH }
});
```

to: 

```js
// Make sure the npm finds this exact version of node in its $PATH.
var binDir = path.dirname(process.execPath);
var PATH = binDir + path.delimiter + process.env.PATH;
var env = Object.assign(process.env, {
  PATH
});
```

`npm i` now works but `cd ../../ && node main.js` exits immediately :(
