# Running `process_exporter` as root

In order to read certain metrics about processes running as other users, the
`process_exporter` will need to run as root. By default, installing perl
modules when running `cpan` as root will install them system-wide visible to
all users. This may not be what is wanted.

They can be installed in a user-specific location in the user's home
directory, even as root.

First create a small environment variable shell script and source it into the
shell:

```sh
$ cat >env.sh
#!/bin/sh
export PERL5LIB="$HOME/perl5/lib/perl5"
export PERL_MM_OPT="INSTALL_BASE=$HOME/perl5"
export PERL_MB_OPT="--install_base $HOME/perl5"
^D

$ . ./env.sh
```

Then you can run `cpan` as normal, and it will install into this user-specific
directory. We'll start by configuring CPAN to prefer `Module::Build`

```sh
$ cpan
cpan> o conf prefer_installer MB
```

While we're at it, setting the default installation policy for build-time
dependencies to `yes` will prevent it from asking us lots of questions during
installation, which just makes the process more awkward.


```sh
cpan> o conf build_requires_install_policy yes
```

Now we can commit the config and install `Module::Build` specially, because
will be required for actually installing the other things.

```sh
cpan> o conf commit

cpan> install Module::Build
cpan> ^D

$ ./install-deps.pl
```
