# README #

Current State: Demo with Test Files and Commands for a Node-RED based PDF processing service

This Node-RED project is at this time meant to be run in local mode. The idea is to run an instance of Node-RED entirely from within a local directory to make it more portable. This also makes running multiple instances of Node-RED at the same time easier. Further development/deployment ideas include serving the whole app with all dependencies in one package, e.g. a Docker container, to make sure it's not dependent on the environment.

### What is this repository for? ###

* Quick summary

The commands in this guide have been applied only on OS X 10.11 El Capitan and 10.10.5 Yosemite, however the links provide further instructions for other OS (Windows, Linux)

* Version

v 0.0.1

* [Learn Markdown](https://bitbucket.org/tutorials/markdowndemo)

### How do I get set up? ###

* Summary of set up

First check the dependencies section to have your system properly set up.

* **Installation & Dependencies**

  1. Clone this repository in a directory of your choice: Since this is a standalone installation there is no need to install node-red explicitely.

  2. [Homebrew](http://brew.sh/index_de.html) has to be installed first as well as [npm](https://changelog.com/posts/install-node-js-with-homebrew-on-os-x) and [pm2](https://www.npmjs.com/package/pm2)

  3. Install [REDIS](https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298) and [mongoDB](https://docs.mongodb.com/v3.0/tutorial/install-mongodb-on-os-x/) package via homebrew:
    

      * `brew install mongodb` *[homebrew](http://brew.sh/index_de.html) has to be installed first

      * `brew install redis` (or skip it now and follow the instructions at "Usage - 1")


  4. Install npm dependencies

    Navigate to your project folder in Terminal and type: `npm install` (if you installed REDIS in the previous section, because the Node-RED instance and dependent tools are started right after)

* **Configuration**

* **Database configuration**

      MongoDB: [Configure MongoDB](http://recipes.item.ntnu.no/using-node-red-mongodb-to-gather-and-publish-data-on-a-raspbe-pi/) - before you can start `mongod` you first have to create the database directory: `sudo mkdir -p /data/db` and `sudo chown "id -u" /data/db` (<-- the "" are backticks)

* **Usage**

  1. In Terminal in your folder type: `npm run redis:start` to start REDIS first. (`npm run redis:stop` to stop and `npm run redis:restart` to restart if problems occur, `npm run redis` to unlink, install and start REDIS)

  2. When running multiple instances in parallel, you can specify a port after you started the kue-dashboard (REDIS has to run) in another Terminal window with `npm run kue-dashboard`:

		```
		npm start -- -p 1885
		```

		To run a specific flow file:

		```
		npm start -- testFlow.json
		```

  3. Or alternatively, just start with `npm start` (this starts the kue-dashboard aswell) and adapt some settings if needed:

      ```
    npm config set pdftoolbox-node-red:nodered_port <port number (default: 1880)>
    npm config set pdftoolbox-node-red:nodered_flows <flows file (default: flows.json)>
    npm config set pdftoolbox-node-red:kue_dashboard_port <port number (default: 3000)>
    npm config set pdftoolbox-node-red:redis_url <url to redis (default: redis://127.0.0.1)>
      ```
  		
      When Ctrl + C is pressed, node-red and the kue-dashboard get both terminated.

  4. Running with PM2 - most comfortable option:

      * type first `npm run redis` to reinstall & start REDIS, then `npm install -g pm2` to install pm2 globally and then `pm2 start app.json` to start the Application and monitor changes & autoreload on Errors

      * type `pm2 [cmd] app.json`, where [cmd] is 'start', 'startOrRestart', 'startOrReload', 'startOrGracefulReload', 'stop', 'restart', 'reload', 'gracefulReload' or 'delete' to perform these operations on the App within pm2

      * type the above commands and replace "app.json" with the name of the individual app from the pm2 overview to target your individual app

      * type `pm2 list` for an overview of all started processes, `pm2 logs [appname]` for the log output ([appname] is usually "nodered-pdftoolbox", please refer to the App name in the `pm2 list`) or list all logs via `pm2 logs --lines 1000`

  5. Open Browser at:
  
      * (http://127.0.0.1:1880/) - to inspect and modify the flows (or use the port number you assigned before)

      * (http://127.0.0.1:3000/) - to inspect and monitor the jobs in the Kue dashboard (or use the port number you assigned before)

      * (http://127.0.0.1:4000/kue) - to inspect and monitor the jobs in the alternative Kue UI
  
      * (http://127.0.0.1:4000/api) - to inspect and monitor the jobs in the classic Kue UI dashboard


  6. Open Finder/other File explorer and navigate to the input directory, located at `<your-project-folder>/pdftoolbox-node-red/input` and add or delete a XML file with PDF to start the process 

  7. Debug information is displayed in the Node Red debug tab in the GUI, in the console, success/error files are saved in the `<your-project-folder>/pdftoolbox-node-red/output` directory.

  8. Logs are saved in daily rotating files in the "log" directory, as well as printed in the console, both using Winston.


* How to run tests
* Deployment instructions

### Contribution guidelines ###

* Writing tests
* Code review
* Other guidelines

### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact