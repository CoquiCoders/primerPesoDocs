# Environments

Our application runs in 3 environments: local, development and staging.

---------------

Every Heroku app runs in at least two environments: on the Heroku platform (we’ll call that **production**) and on your local machine (**development**). If more than one person is working on the app, then you’ve got multiple development environments - one per machine, usually. Usually, each developer will also have a test environment for running tests.

This separation keeps changes from breaking things. You write code and check the site in development, but you run your tests in the test environment to keep them from overwriting your development database.

Similarly, you might have broken features in your development environment most of the time, but you only deploy working code to production.

Unfortunately, this approach breaks down as the environments become less similar. Windows and Macs, for instance, both provide different environments than the Linux stack on Heroku, so you can’t always be sure that code that works in your local development environment will work the same way when you deploy it to production.

The solution is to have a **staging** environment (third environment) that is as similar to production as is possible. This can be achieved by creating a second Heroku application that hosts your staging application. With staging, you can check your code in a production-like setting before having it affect your actual users. As you already deploy with git, setting up and managing these multiple remote environments is easy.

