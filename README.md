# Sono-Bang

MP3 playlist <Need 10 second puch line>

# Setup

Clone the repository on your local machine.  Once finished, create a file called 

`.env` 

inside root project folder.  This file will not be checked in for security purposes and referenced in the .gitignore file so you don't have to worry about checking in this file.  

Add the following line:

```
CLEARDB_DATABASE_URL=mysql://root@localhost:3306/sonobang?reconnect=true

#Enable Drop and recreate tables when unit tests are run (this will override to `test` when unit test are executing)
NODE_ENV=development;
```

Save the file.

Be sure to have mysql server up and running.  On Mac its `mysql.server start`.  If you plan on doing development, its advisable to run Mysql as a service and keep it on all the time.  Otherwise, you have to start up this service everytime you need to test.

With your favorite Mysql IDE, create a schema called `sonobang` 

Install of the dependencies for this project.

`npm install`

# Test

This project is a set of unit tests.  To run them, simply type in the following command:

`npm test`

Run Seed Data(Optional)

`../node_modules/.bin/sequelize db:seed:all`

# Individual Tests

Sometimes, to avoid clutter, one may want to individually test. Be careful to set timeout for long running tasks that last longer than 2 seconds.  npm test will set this flag, however, running indvidual tests, please set the `--timeout` in milliseconds otherwise for slow internet connections, your tests may fail.

`mocha tests/aws/*.js --timeout 15000`

# Sequelize ORM

Sequelize is used to simplify Mysql wrapping.  These tools simplify reading, writing, updating and deleting data from database.  The Sequelize-cli provides a tool to simplify writing boiler plate code used in Sequelize operation esecailly for `migrations`.  Whenever its needed to add a field to a table, one should use this cli rather than to edit Mysql directly.  That way, when someone checks in their code, the other developer's Mysql instances are automatically synched when the test runs. Instead of trying to memorize sequelize, its easier to execute a few command via Command-Line. Also, if there is a problem with the database schema, simple drop and recreate the schema:

```

drop database sonobang

create database sonobang

npm test

```

The database tables should automatically regenerate and your tests should pass.



Here are some reminder steps in order for command line syntax:

Before doing any CLI, please refer to this link for further instructions:

[Migrations](http://docs.sequelizejs.com/manual/tutorial/migrations.html#migration-skeleton)

`cd db`

## 1 - Create a new model

```
../node_modules/.bin/sequelize model:generate --name products --attributes item_id:integer
```

Warnining - Do not edit the model class directly.  The migration will alter this file accordingly. One should exist per table.

When creating a new model, a migration will automatically be created.  Make the necessary changes.

## 2 - Commit changes to Mysql

Once ready, commit structure to Mysql.  Migrations can be undone in case any mistakes happen.  See [docs](http://docs.sequelizejs.com/manual/tutorial/migrations.html#migration-skeleton)

```
../node_modules/.bin/sequelize db:migrate

undo

../node_modules/.bin/sequelize db:migrate:undo
```

## 3 - Create a New Migration from existing model

As time goes by, multiple developers will need to make changes to the same table.  A migration is a developer's database snapshot to make the current release function correctly.

```
../node_modules/.bin/sequelize migration:create --name products

```

## 4 - Generate Seed Data

Seed data is initial data that is inserted into the database

```
../node_modules/.bin/sequelize seed:generate --name composer-user
```

To commit seed data to Database
```
../node_modules/.bin/sequelize db:seed:all
```


Thats it!  That concludes the project.  Use this project as an example ORM for other projects.  Any question please feel free to ask!





