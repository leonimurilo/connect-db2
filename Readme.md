connect-db2
===========

An [IBM DB2](http://www.ibm.com/analytics/us/en/technology/db2/) session store for [express.js](http://expressjs.com/).

Setup
-----

```sh
npm install connect-db2 express-session --save
```

Pass the `express-session` store into `connect-db2` to create a `Db2Store` constructor.

```js
var session = require('express-session');
var Db2Store = require('connect-db2')(session);

var options = {
	host: 'localhost',
	port: 50000,
	user: 'db2user',
	password: 'password',
	database: 'BLUDB'
};

var sessionStore = new Db2Store(options);
app.use(session({
    store: sessionStore,
    secret: 'keyboard cat'
}));
```

### Using a DSN
Altenatively if you prefer, you can use the full DSN string in the config instead:

```js
var session = require('express-session');
var Db2Store = require('connect-db2')(session);

var options = {
    dsn: 'DATABASE=BLUDB;HOSTNAME=loclhost;PORT=50000;PROTOCOL=TCPIP;UID=db2user;PWD=password;'
};

var sessionStore = new Db2Store(options);
app.use(session({
    store: sessionStore,
    secret: 'keyboard cat'
}));
```

### Using an existing connection
```js
var session = require('express-session');
var Db2Store = require('connect-db2')(session);
var ibmdb = require('ibm_db');

var dsn = 'DATABASE=BLUDB;HOSTNAME=loclhost;PORT=50000;PROTOCOL=TCPIP;UID=db2user;PWD=password;';
var options = {};
var conn = ibmdb.openSync(dsn);

var sessionStore = new Db2Store(options, conn);
app.use(session({
    store: sessionStore,
    secret: 'keyboard cat'
}));
```

Closing the session store
-------------------------

To cleanly close the session store:
```js
sessionStore.close();
```

Options
-------

Here is a list of all available options:
```js
var options = {
	host: 'localhost',// Host name for database connection.
	port: 50000,// Port number for database connection.
	user: 'db2user',// Database user.
	password: 'password',// Password for the above database user.
	database: 'BLUDB',// Database name.
	expiration: 2592000,// The maximum age of a valid session; milliseconds.
	schema: {
		tableName: 'sessions',
		columnNames: {
			session_id: 'session_id',
			expires: 'expires',
			data: 'data'
		}
	},
    allowDrop: false // If true, allows dropping the session table by calling sessionStore.dropDatabaseTable(), default: false
};
```

Contributing
------------

This is still a very young project and there are a number of ways you can contribute:

* **Improve or correct the documentation** - All the documentation is in this readme file. If you see a mistake, or think something should be clarified or expanded upon, please [submit a pull request](https://github.com/wallali/connect-db2/pulls/new)
* **Add test cases** - There is need for several more test cases and unit-tests to cover uncovered areas of code. It is a great way to get started with this project.
* **Report a bug** - Please review [existing issues](https://github.com/wallali/connect-db2/issues) before submitting a new one; to avoid duplicates. If you can't find an issue that relates to the bug you've found, please [create a new one](https://github.com/wallali/connect-db2/issues).
* **Fix a bug** - Have a look at the [existing issues](https://github.com/wallali/connect-db2/issues) for the project. If there's a bug in there that you'd like to tackle, please feel free to do so. Please submit a test case with that covers your code. After you've done all that, you can [submit a pull request](https://github.com/wallali/connect-db2/pulls/new) with your changes.
* **Request a feature** - Again, please review the [existing issues](https://github.com/wallali/connect-db2/issues) before posting a feature request. If you can't find an existing one that covers your feature idea, please [create a new one](https://github.com/wallali/connect-db2/issues). This project is maintained in my free time, chances are you will need to implement your feature idea yourself or wait till I get the chance to do it.

Before you contribute code, please read through at least some of the source code for the project. I would appreciate it if any pull requests for source code changes follow the coding style of the rest of the project. Use ```npm run lint``` to lint your code before submission.

Configure Local Dev Environment
---------------------------
### Step 1: Get the Code

First, you'll need to pull down the code from GitHub:
```
git clone https://github.com/wallali/connect-db2.git
```

### Step 2: Install Dependencies

Second, you'll need to install the project dependencies as well as the dev dependencies. To do this, simply run the following from the directory you created in step 1:
```
npm install
```

### Step 3: Set Up the Test Database

Now, you'll need to set up a local test database or create a free dashDB instance on [IBM Bluemix](https://console.ng.bluemix.net/catalog/services/dashdb/):

```js
{
	host: 'localhost',
	port: 50000,
	user: 'db2user',
	password: 'password',
	database: 'BLUDB'
};
```
*The test database settings are located in [test/config.js](https://github.com/wallali/connect-db2/blob/master/test/config.js)*

Alternatively, you can provide custom database configurations via environment variables:
```
DB_HOST="localhost"
DB_PORT="50000"
DB_USER="db2user"
DB_PASS="password"
DB_NAME="BLUDB"
```

or a DSN via environment variables:
```
DB_DSN="DATABASE=BLUDB;HOSTNAME=loclhost;PORT=50000;PROTOCOL=TCPIP;UID=db2user;PWD=password;"
```

### Running Tests

With your local environment configured, running tests is as simple as:
```
npm test
```

Debugging
---------

`connect-db2` uses the [debug module](https://github.com/visionmedia/debug) to output debug messages to the console. To output all debug messages, run your node app with the `DEBUG` environment variable:
```
DEBUG=connect:db2 node your-app.js
```
This will output debugging messages from `connect-db2`.

License
-------

[MIT](https://github.com/wallali/connect-db2/blob/master/LICENSE)