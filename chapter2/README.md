## How to get started?

**How to Run**

* Dependencies
* Postgresql 9.3 - DB
* NodeJS 0.10
* Redis - Sessions
* Mandrill - E-Mail
* TravisCI - CI


**Fresh Install**

***Cloning the Repository***

1. The first step is to clone the PrimerPeso source code to your local computer.

2. Open an empty terminal window and if you have not done so already clone PrimerPeso by running:

git clone git@github.com:codeforamerica/primerpeso.git

3. Now change directory into primerpeso's source code folder.

cd primerpeso

***Installing node dependencies***

1. The second step is to install the dependencies and open source libraries primerpeso uses.

2. Simply run this command while in the primerpeso source code folder:

npm install

3. After it's done your project should have a new folder called node_modules.

***Installing and Initializing the local database***

1. We are using PostgreSQL as our database management system. In order to run primerpeso you need to have a local version of PostgreSQL running on your computer.

2. For Mac OSX users we reccommend installing postgresapp. After installing run this line to link the database to the psql command:

echo "export PATH=$PATH:/Applications/Postgres.app/Contents/Versions/9.3/bin" >> ~/.bash_profile

3. We need a new terminal window for this line to take effect. Go ahead and open a new window and change directory to the primerpeso folder.

4. Now open the Postgresapp. (You should have a black elephant on your computer's top bar) Note: If you have the elephant on the top bar Postgres is already running Note: You have to open Postgres every time you want to run the project.

5. We need to initialize the database so run this command in the terminal window:

psql -f config/init.sql

6. Now run this command to load some sample data into the database:

psql primerpeso < config/dump.sql

***Installing redis:***

1. We need to install Redis, make sure you have Homebrew installed and run this command in a terminal window:

brew install redis

***Creating your environment variables file***

1. We use environment variables to connect to our local database and tell PrimerPeso to run in "Development" mode. We specify these things in a special file named ".env" (Yes, with the period):

2. Create an empty file named ".env" touch .env
Copy the contents of ".env_example" into it cat .env_example > .env
3. If you don't know your terminal username run whoami to find it
4. Open the ".env" file in a text editor.
5. Find the "DATABASE_URL" variable, change the part where it says "username" into your terminal username
Running PrimerPeso

***We use a tool called gulp to run the project. Let's install that now.***

npm install -g gulp

***Finally to run the project run this command:***

1. gulp
2. Your project should be available at localhost:3737

**Already Installed**

1. In a new terminal window, change directory into the PrimerPeso folder

cd path/to/folder/primerpeso

2. After you change directory to the PrimerPeso folder, run this command:

gulp

***Importing and Exporting DB.***

**Dump**

pg_dump bizwallet --format=c --file=db_dump_a.tar.gz --no-owner

**Restore**

From Heroku = dropdb bizwallet; heroku pg:pull DATABASE_URL bizwallet; gulp migrate
From Dump = dropdb bizwallet; createdb bizwallet; pg_restore db_dump_a.tar.gz --clean --dbname=bizwallet --create

**Common Issues**

Some common issues that could give you errors when you run the project, and how to solve them

* New dependencies were added (run npm install)
* Make sure you are in the primerpeso folder (run cd path/to/primerpeso)*
* @MrMaksimize or @chrisrodz broke something (ping us to fix it)



