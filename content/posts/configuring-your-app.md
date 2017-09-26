+++
    title = "Configuring Your App"
    date = 2017-09-26T01:50:39+05:30
    draft = false
    Description = "Ember App Configuration"
    Tags = ["Configuring", "Ember"]
    Categories = ["Ember"]
+++

Ember CLI ships with support for managing your application's environment. Ember CLI will setup a default environment config file at <code>config/environment</code>. Here, you can define an <code>ENV</code> object for each environment, which are currently limited to three: development, test, and production.

The ENV object contains three important keys:

   - <code>EmberENV</code> can be used to define Ember feature flags [(see the Feature Flags guide)](https://guides.emberjs.com/v2.15.0/configuring-ember/feature-flags/).

   - <code>APP</code> can be used to pass flags/options to your application instance.

   - <code>environment</code> contains the name of the current enviroment (<code>development</code>, <code>production</code> or <code>test</code>).

You can access these environment variables in your application code by importing from <code>your-application-name/config/environment</code> .

For example:

```
import ENV from 'your-application-name/config/environment';

if (ENV.environment === 'development') {
  // ...
}
```


### CONFIGURING EMBER CLI

In addition to configuring your app itself, you can also configure Ember CLI. These configurations can be made by adding them to the <code>.ember-cli</code> file in your application's root. Many can also be made by passing them as arguments to the command line program.

For example, a common desire is to change the port that Ember CLI serves the app from. It's possible to pass the port number from the command line with <code>ember server --port 8080</code>. To make this configuration permanent, edit your <code>.ember-cli</code> file like so:

```
{
  "port": 8080
}
```
For a full list of command line options, run <code>ember help</code>.


#### We can have different configuration based on build environment

```
### environment.js

module.exports = function(environment) {
  var ENV = {
    modulePrefix: 'APP',
    environment: environment,
    baseURL: '/',
    locationType: 'auto',
    EmberENV: {

     }

     if (environment === 'development') {
        // Dealing with deprecations
        RAISE_ON_DEPRECATION = true
        LOG_STACKTRACE_ON_DEPRECATION = true

        APIENDPOINT = 'http://example.com'
      }  

      if (environment === 'production') {
        APIENDPOINT = 'http://someotherexample.com'
      }            
   }

   ```

### Compiling Assets
Ember CLI uses the Broccoli assets pipeline. 

The assets manifest is located in the <code>ember-cli-build.js</code> file in your project root (not the default ember-cli-build.js). It is responsible for recompiling the assests when a project file changes.

Example: 
```
// ember-cli-build.js
var EmberApp = require('ember-cli/lib/broccoli/ember-app');

module.exports = function(defaults) {
  var app = new EmberApp(defaults, {
    minifyJS: {
      enabled: false
    },
    minifyCSS: {
      enabled: false
    }
  });

  //...
  return app.toTree();
};
```