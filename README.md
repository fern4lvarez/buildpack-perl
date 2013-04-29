Perloku
=======

Deploy Perl applications in seconds.

## Step 1

Write an app (`app.pl`):

```perl
#!/usr/bin/env perl
use Mojolicious::Lite;

get '/' => sub {
  my $self = shift;
  $self->render('index');
};

app->start;
__DATA__

@@ index.html.ep
% layout 'default';
% title 'Welcome';
Welcome to the Mojolicious real-time web framework!

@@ layouts/default.html.ep
<!DOCTYPE html>
<html>
  <head><title><%= title %></title></head>
  <body><%= content %></body>
</html>
```

## Step 2

Create a `Makefile.PL` with your dependencies:

```perl
use strict;
use warnings;

use ExtUtils::MakeMaker;

WriteMakefile(
  NAME         => 'app.pl',
  VERSION      => '1.0',
  AUTHOR       => 'Magnus Holm <judofyr@gmail.com>',
  EXE_FILES    => ['app.pl'],
  PREREQ_PM    => {'Mojolicious' => '2.0'},
  test         => {TESTS => 't/*.t'}
);
```

## Step 3

Create an executable file called `Perloku` which runs a server on the port
given as an enviroment variable:

```sh
#!/bin/sh
./app.pl daemon --listen http://*:$PORT
```

Test that you can start the server:

```sh
chmod +x app.pl
chmod +x Perloku
```

## Step 4

Create a `Procfile`:

```
web: ./Perloku
```

## Step 5

Push & Deploy:

```sh
git init
git add .
git commit -m "Initial version"
cctrlapp APP_NAME create custom --buildpack http://github.com/fern4lvarez/buildpack-perl.git
cctrlapp APP_NAME/default push
cctrlapp APP_NAME/default deploy
```

Watch:

```
Counting objects: 5, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 808 bytes, done.
Total 5 (delta 0), reused 0 (delta 0)

-----> Receiving push
-----> Custom buildpack provided
-----> Vendoring Perl
       Using Perl 5.16.2
-----> Installing dependencies
       --> Working on /srv/tmp/builddir
       Configuring /srv/tmp/builddir ... OK
       ==> Found dependencies: Mojolicious
       --> Working on Mojolicious
       Fetching http://www.cpan.org/authors/id/S/SR/SRI/Mojolicious-3.97.tar.gz ... OK
       Configuring Mojolicious-3.97 ... OK
       Building Mojolicious-3.97 ... OK
       Successfully installed Mojolicious-3.97
       <== Installed dependencies for /srv/tmp/builddir. Finishing.
       1 distribution installed
       Dependencies installed
-----> Building image
-----> Uploading image (13M)
       
To ssh://APP_NAME@cloudcontrolled.com/repository.git
 * [new branch]      master -> master

```

[Enjoy!](http://perlcctrlexample.cloudcontrolled.com)
