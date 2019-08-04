# scp-uploader
simple PHP script to upload several files to a remote ssh server, considering the path of each file

## Purpose or *my typical usecase*

I created this just to upload several files to a remote server after a new version of the app is ready.

For instance:

- app is at tag `v1.0.0`
- you create a new branch, introduce some changes, and go back to master
- you merge the new branch into master and decided to publish this changes
- you create a new tag for the app, lets say it was a big change, so lets increase minor version:
```
git tag v1.1.0
```
- now you have several files (around 36), in various dirs that you need to update:
```
git diff --name-only v1.1.0 v1.0.0

.env.example
.env.testing
.gitignore
app/Console/Commands/ClearJobsQueues.php
app/Console/Commands/Import.php
app/Console/Kernel.php
app/Exceptions/WrongJobStatus.php
app/Http/Controllers/ExpController.php
app/Http/Controllers/FeedbackController.php
app/Http/Controllers/ImportController.php
app/Http/Controllers/LoginController.php
app/Http/Controllers/MailController.php
app/Http/Controllers/RecoveryController.php
app/Http/Controllers/StatsController.php
app/JobLog.php
app/Jobs/ProcessSigZip.php
app/Jobs/ProcessSqlZip.php
app/Lib/Output/BufferedTeeOutput.php
app/Lib/Utils/Utils.php
app/Mail/DenialNotification.php
app/Mail/InfoRequest.php
app/Mail/ReceiptProof.php
app/Providers/AppServiceProvider.php
bootstrap/app.php
config/database.php
config/mail.php
config/queue.php
database/migrations/2019_06_25_195351_create_queue_tables_mysql.php
database/migrations/2019_06_27_093147_create_jobs_loggin_table.php
phpunit.xml
resources/views/emails/inforequest.blade.php
routes/web.php
tests/ImportControllerTest.php
tests/RecoveryTest.php
tests/fixtures/exports-sigeco.zip
tests/fixtures/l52smaller.mdb.zip
```
- obviously, uploading all of this by hand is not recommended

That's why:

## Using the script

The script is [uploadfiles](./uploadfiles). It has a simple usage:

    php uploadfiles <user>@<host>:</destination/folder/> <files...>

So if you are to call it with the above list you could do:

    $ php uploadfiles root@exmpl.com:/var/www/ `git diff --name-only v1.1.0 v1.0.0`

This will invoque it with all of the files. And every thing will be copied keeping the full path.

## Dependencies

This PHP script invokes `scp`, a widely spread tool that comes with the regular `ssh` installation. So if I have to say:

- PHP >= 5
- SSH, which ever version works for you.

## Config

The are no config options for the script *per se*. Any special config is done in the SSH config file (i.e. `~/.ssh/config`).

In there you should configure the hosts specific options. How to do that is out of the scope of this `README`, sorry. But a start point can be this [post](https://jjtriff.github.io/blog/fun-with-dolphin-and-ssh) of mine ;).
