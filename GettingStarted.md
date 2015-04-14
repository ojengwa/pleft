### Introduction ###

These instructions will guide you through setting up a local instance of Pleft that you can work with.

Feel free to comment here or contact our [public mailing list](https://groups.google.com/forum/#!forum/pleft) at [pleft@googlegroups.com](mailto:pleft@googlegroups.com) if you are stuck.

### System requirements ###

  * Django 1.3 or later. See Django's [Quick install guide](https://docs.djangoproject.com/en/1.3/intro/install/).
  * A Java runtime environment (for plovr). If this is not provided by your OS, see [Java.com](http://java.com/).

### Installation ###

  1. [Check out the source code](http://code.google.com/p/pleft/wiki/Source?tm=4).
  1. Enter into the newly created pleft/ directory: cd pleft/
  1. Make a new directory named external in the pleft/ directory: mkdir external
  1. Download the [most recent plovr build](http://code.google.com/p/plovr/downloads/list) to the external/ dir.
  1. Symlink or copy external/plovr-[[code](code.md)].jar to external/plovr.jar. For example, if you run Unix or Linux, use a command like this (after cd external/):
```
ln -s plovr-c047fb78efb8.jar plovr.jar
```
  1. Create a file pleft/local\_settings.py, based on one of the examples in that directory. If you just want to test Pleft, copy pleft/local\_settings.py.debug to pleft/local\_settings.py to do that. You need to edit the file to modify some paths. Especially database information (i.e. DATABASE\_NAME) and SECRET\_KEY need to be changed. The SQLite database will be created automatically.
  1. Create the proper tables in the database:
```
python pleft/manage.py syncdb
```

### Running ###

Run both de Pleft and the Plovr debug servers concurrently (i.e. open two terminal windows, and run one command in each):
```
python pleft/manage.py runplovr
python pleft/manage.py runserver
```

In debug mode, emails are written to the console instead of being sent.
If you need a link that was provided this way, note that characters like
the = are escaped. So you might for example need to replace =3D with =.

### Translations ###

If you want to test or deploy translations, install gettext and run:
```
python pleft/manage.py mo
```

### Deployment ###

If you want to deploy Pleft, compile the JavaScript files to .cjs using:
```
mkdir static/scripts
python pleft/manage.py compile
```


#### Note for MySQL users ####

If you are using MySQL, make sure that the database is created using:
```
CREATE DATABASE name CHARACTER SET utf8
```
Also, for the user system to work, the email address field must be made case sensitive, using:
```
ALTER TABLE plauth_user MODIFY email VARCHAR(255) COLLATE utf8_bin
```

### Publishing changes ###

Create commits for your changes. See _[Making changes](http://schacon.github.com/git/gittutorial.html#_making_changes)_ for help.

There are two ways to publish changes to the code:
  * Create a fork of the repository and push your changes to it. See [Fork A Repo](http://help.github.com/fork-a-repo/). This makes it easy to review your code.
  * Create a patch file using the command 'git format-patch origin/master --stdout > filename.patch'.