# Environments and Development Process.


Every Heroku app runs in at least two environments: on the Heroku platform (we’ll call that **production**) and on your local machine (**development**). If more than one person is working on the app, then you’ve got multiple development environments - one per machine, usually. Usually, each developer will also have a test environment for running tests.

This separation keeps changes from breaking things. You write code and check the site in development, but you run your tests in the test environment to keep them from overwriting your development database.

Similarly, you might have broken features in your development environment most of the time, but you only deploy working code to production.

Unfortunately, this approach breaks down as the environments become less similar. Windows and Macs, for instance, both provide different environments than the Linux stack on Heroku, so you can’t always be sure that code that works in your local development environment will work the same way when you deploy it to production.

The solution is to have a **staging** environment (third environment) that is as similar to production as is possible. This can be achieved by creating a second Heroku application that hosts your staging application. With staging, you can check your code in a production-like setting before having it affect your actual users. As you already deploy with git, setting up and managing these multiple remote environments is easy.

In order to successfully manage your code and deployments, we have used the following workflow:

### General Rules
* Create a master and a production branch of the repository.
* Master branch = Staging Environment = Where the code goes in first.
* Production branch = Prod Enviornment = Only working code goes here.
* Never push to the master branch;
* Never push to the production branch;
* A developer may NEVER commit his / her own code into master or production branches.

### Development Process
* Before working on a feature, the developer clones the repository:
  * `git clone git@github.com:codeforamerica/primerpeso.git`
  * OR
  * if she has the repo locally already, `git fetch origin`;
* Next, she runs `git checkout origin/master` to get the repo in a HEADLESS state, based directly on the master branch.
* `git checkout -b my-awesome-feature` will check out a local feature branch for the developer, and she can begin her work.
* Periodically, she will run `git fetch origin && git merge origin/master` in order to keep up to date with any code that has gone into the upstream master.
* Periodically, she will commit her code and push it (`git add -p`, `git commit -m`, `git push origin my-awesome-feature`)
* When she is ready for the rest of the team to see her code, she will make a Pull Request from her branch in to master.
* Another developer will review her pull request and make comments as needed.  In addition Travis-CI will run any tests to make sure that Pull Request does not break them.
* Another developer will merge her pull request.
* Once her code passes all the tests, Travis-CI will deploy it to the staging environment at [http://bizwallet-staging.herokuapp.com](http://bizwallet-staging.herokuapp.com);
* The team will review and when a production push is scheduled, a member will create a pull request from Master > Production.
* Travis-CI will run tests against that pull request, and if they all pass another developer will merge.
* Once Travis-CI completes another test run, the code will be deployed to production.
