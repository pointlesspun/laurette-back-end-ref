# Laurette van Zanten Thesis Back-end

This directory contains the back-end code for Laurette van Zanten's thesis experiment. The experiment is based on a game which communicates the results to a back-end server. The back-end server is a Node.Js/Express stack with a MySQL or Postgres database adapter. 

## Dependencies & Configuration

The following steps need to be taken in order to run the back-end server:

* Clone the project from https://github.com/pointlesspun/laurette-back-end-ref

* To run the back end make sure node-js, npm, mysql and/or postgres are installed. Visual studio code is highly recommend.

* Perform an NPM install in the root directory of the project (type `npm install` on the command line).

* Create a database.
  * MySql: Log in to the MySql server. Run mysql.exe in the bin  directory. Use the -U and -P parameters for the user / password.   
  * You might need give additional credentials to your local mysql user by invoking from the mysql server commandline: `ALTER USER 'your-user-name' IDENTIFIED WITH mysql_native_password BY 'your-password';`
  * To create a MySql database run the `createmysqldb.sql` script (in case of MySQL installation). Note that this script will drop any existing 'laurette_db' databases and the associated data. Run `mysql -u user_name -p < "C:\path_to\createmysqldb.sql"`
  * Postgres:Start the server `pg_ctl -D "C:\path_to\PostgreSQL\12\data" start`  
  * Log in to the Postgres server and either use `psql -d laurette_db -a -f c:\path_to\createpostgresdb.sql`

* Define the following environment variables (set VARNAME=xxx on the windows command line):
  * process.env.PORT: if not defined the default port 3000 will be used
  * process.env.DATABASE_URL: set the connection string for the database
  * process.env.GAME_ADMIN: admin name for back-end game admin functions
  * process.env.GAME_ADMIN_PASSWORD: admin password for back-end game admin functions
  
## Running the server

Both the Postgres and MySQL server are run off one and them same implementation, . To run the server and assuming all dependencies are in place (npm, node, mysql or postgres) type on the commandline:

```node server.js ./postgres-adapter.js postgresql://postgres_user:postgres_admin@localhost:5432/laurette_db``` 

or 

```node server.js ./mysql-adapter.js mysql://mysql_user:mysql_admin@localhost:3306/laurette_db```

In the case of the latter, if successful the following will be output on std out:

```
{"message":"@MACHINE-NAME Connecting to db via mysql://mysql-user:mysql-password@localhost:3306/laurette_db","level":"info"}
{"message":"connecting to : \"mysql://mysql-user:mysql-password@localhost:3306/laurette_db\".","level":"info"}
{"message":"configuring CORS","level":"info"}
{"message":"Server running on port 3000","level":"info"}
{"message":"connection to database established at: Sat May 02 2020 14:39:32 GMT+0200 (Central European Summer Time)","level":"info"}
```
## Connecting the client to the server

First run the server as described above.  

In Unity, to have the client send data to server there are some scenarios to consider:

* When testing an individual scene (not starting the login) add a new game object with a `webcom` component. Set the following parameters:
  * Url: http://localhost:3000 (make sure there is no trailing slash)
  * User Name: tst1 (or any other valid user account -- don't use 'guest', 'guest' is actively ignored by the client)
  * Password: test_pwd1
  * Scene: whatever the scene build id is
  * Make sure the object containing the webcom is tagged as "WebCom"
  * Set the 'Login on start' option as true

 * A webcom component may already be present in the gamestate object. If so, you'll only need to change the parameters as aforementioned.
 
 * When testing the full game, simply start with the login screen no additional work should be required.  

To see if the communication works, check the console. If no errors pop up the data should end up in the database.

## Misc

 * Download the postgres db game data table  `\COPY gamedata TO 'gamedata_export.csv' WITH (FORMAT csv, DELIMITER ',',  HEADER true);`


## changelog 24 May 2020

* Expanded readme,
* Non null, empty orders are now accepted in order to track progress

## Changelog 11 May 2020

* Removed the separate servers (postgres-server and mysql-server) in favor of one server (server.js)
* Changed timestamps in the schema to real values. 
* Renamed 'session' to 'scene' in the db schema.
* Updated the readme.
* SQLite3 has been deprecated. 
