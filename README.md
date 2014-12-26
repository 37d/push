raix:push Push notifications
=========

Push notifications for cordova (ios, android) browser (Chrome, Safari, Firefox) - One unified api on client and server.

Status:
* [x] APN iOS
* [x] GCM Android
* [x] APN Safari web push (untested)
* [x] GCM Chrome OS (untested)
* [x] Firefox OS (untested)
* [ ] BPS Blackberry 10
* [ ] MPNS Windows phone 8
* [ ] MPNS Windows 8
* [ ] ADM Amazon Fire OS
* [ ] Meteor in app notifications

## Common api
```js
    // Push.debug = true; // Add verbosity

    Push.send({
        from: 'push',
        title: 'Hello',
        text: 'world',
        query: {} // Query the appCollection
        // token: appId or token eg. "{ apn: token }"
        // tokens: array of appId's or tokens
    });

    Push.addListener();

    // Security
    // Push.allow
    // Push.deny
```
*The client-send branch is WIP of allow/deny rules in order to allow the user to send push messages to other users directly from the client*

## Server api

```js
    Push.init({
        gcm: {
            apiKey: 'xxxxxxxxxxxxx'
        },
        apn: {
            // setting this on client throws security error
            passphrase: 'xxx',
            // pem files are placed in the app private folder
            certData: Assets.getText('apnProdCert.pem'),
            keyData: Assets.getText('apnProdKey.pem'),
        },
        production: true, // use production server or sandbox
    });

    // Internal events
    Push.addListener('token', function(currentToken, newToken) {
        // Token is { apn: 'xxxx' } or { gcm: 'xxxx' } or null
        // if newToken is null then the currentToken is invalid
        // if newToken is set then this should replace the currentToken
    });    
```

## Client api
```js
    // Common client api
    Push.init({
        gcm: {
            // Required for Android and Chrome OS
            projectNumber: 'xxxxxxxxxxxxxxxxxx'
        },
        apn: {
            // Only required if using safari web push, not required
            // for iOS / cordova
            websitePushId: 'com.push.server'
            webServiceUrl: 'http://some.server.com'
        },
        bagde: true,
        sound: true,
        alert: true
    });

    Push.id(); // Unified id - not a token

    Push.setBadge(count); // ios specific - ignored everywhere else

    // Internal events
    Push.addListener('token', function(token) {
        // Token is { apn: 'xxxx' } or { gcm: 'xxxx' }
    });

    /*
    error { type: 'gcm', error: error }
    register
    startup
    badge
    */
```


Kind regards

Morten (aka RaiX)
