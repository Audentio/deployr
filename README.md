# DeployrJS

### What?
DeployrJS allows quickly bootstrapping a deployment server that listens to github web hooks
and does whatever you want whenever an action occurs. This includes system commands and/or arbitrary JS

### How?

**1. Install** `npm install --save deployrjs`

**2. take over the planet**
```javascript
const DeployrJS = require('deployrjs');
const exec = require('child-process-promise').exec;

const deployment = new DeployrJS({
    // this is your github secret key.
    // Not passing this will disable verification
    key: #####,

    // port to listen on
    port: 4000,
});

deployment.listen((action, pull) => {
    if (action.type !== 'push') return;

    pull()
        .then(() => {
            // At this point, files are identical to git repo
            // Let's re-run a build script

            console.log('Running build script...');
            return exec('npm run build');
        })

        .catch(err =>
            console.error('failed!');
        );
});
```

use Promise wrappers for async functions.
e.g. https://www.npmjs.com/package/fs-promise
