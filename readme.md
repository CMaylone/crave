# Crave

Locate and require node components (controllers, models, etc) dynamically within your application.

Crave allows you to structure a server's files how you want without the extra burden of manually requiring files.  For example, you may have this common file structure:

```
/app/controllers/user_controller.js
/app/controllers/device_controller.js

/app/models/user_model.js
/app/models/device_model.js

/app/test/user_test.js
/app/test/device_test.js
```

Which, using Crave, could be restructured to look like this:

```
/app/user/user_controller.js
/app/user/user_model.js
/app/user/user_test.js

/app/device/device_controller.js
/app/device/device_model.js
/app/device/device_test.js
```
This new file structure groups the server's features so they are portable from one project to another.  In addition, debugging and development becomes easier because you are no longer hunting down files that may interact with one another.

**Current Status:** In Development.

# Getting Started

Install Crave using npm and save it as a dependancy in your package.json.

```javascript
npm install crave --save
```

You can require Crave just like every other node.js module.

```javascript
var crave = require('crave');
```

Controllers, modules, and other files you wish to require before starting your server should be structured like this:

```javascript
// This is the Crave type, it tells crave when and 
// if to load this file.  You can use any text you want.
// ~> Controller

// Export a function whose parameters are global values 
// needed in the controller's logic.
module.exports = function (app, config) {

  // Place your controller logic inside this method.

  // An example route that sends "Hello"
  app.get('/', function (req, res, next) {
    res.send("Hello");
  });
};
```

Use Crave's directory method to require your files dynamically.  They will be required in the order you specify and will recieve the parameters you pass.

```javascript
var crave = require('crave'),
    express = require('express');

// Create an express application object.
var app = express();

// Create a method to start the server.
var startServerMethod = function(err) {
  if(err) return console.log(err);

  var server = app.listen(3000, function() {
    console.log("Listening on http://127.0.0.1:3000");
  });
}

// Define the directory where Crave will search for files in.
// Crave will do a recursive search, looking in children folders.
var directoryToLoad = '/path/to/my/app/files';

// Define an ordered list of file types for Crave to load.
// This is the Crave type we just talked about in the 
// earlier example:  " // ~> Controller "
var types = [ "controller" ];

// Crave will now load the files in the given directory with the specified types.
// Each file will recieve the application and configuration objects as parameters.
// You can pass any number of parameters into the files being loaded by continuing
// to overload the crave.driectory method.  Once Crave has loaded all the files
// the callback method will be called, aka startServerMethod.
crave.directory(directoryToLoad, types, startServerMethod, app, config);
```

# Documentation

Further documentation can be found in the [wiki](https://github.com/ssmereka/crave/wiki).


###[MIT License](http://www.tldrlegal.com/license/mit-license "MIT License")
