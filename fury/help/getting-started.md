### Quickstart

To install Fury quickly, just run `curl -Ls get.fury.build | sh`, or execute a buildfile with `./fury install`.

# Getting started

Fury works natively on Linux and Mac OS X, and with all modern Intel and ARM processor architectures. Windows is
supported through the [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/) (WSL). If
you use Windows but have not yet tried WSL, first [install it](/help/wsl) and then continue from your new Linux
terminal.

## Try Fury

### Launching Fury from a buildfile

If you already have a project with a Fury build, usually a file in the project's root directory called `fury`,
the easiest way to start using it is to simply execute the build, by calling,
```sh Bash
./fury
```

This will run Fury on that buildfile, if necessary downloading Fury and Java first.

This is possible because (in addition to containing a project definition) the file contains a small Bash script
to download and run the latest version of Fury. You can learn more about
[how Fury buildfiles work](/help/launch), including some of their security features and limitations.

## Installing Fury

By default, executing a Fury buildfile will download and run it from a temporary directory, but will not
install it. To install it, just specify the `install` subcommand,
```sh Bash
./fury install
```
and it will be installed for the current user, into `~/.local/bin/fury` so that it can subsequently be invoked
with just,
```sh Bash
fury
```
from a project directory.

The `~/.local/bin` directory is normally already on your `PATH` environment variable, which means that the
`fury` command can be invoked from any shell. If not, then the following should be added at the end of your
`.bashrc` or `.zshrc` files, depending on which shell you use:
```sh Bash
export PATH="$HOME/.local/bin:$PATH"
```
and `fury` will be available on any _new_ shell. (So you can't use it straight away in the current shell.)

The executable `fury` file contains everything necessary to run, including the Scala compiler, and does not
require any other files to start, though it will make use of cache and configuration directories (typically
`~/.cache` and `~/.config`, unless your [XDG configuration variables](/help/locations) specify otherwise).

### Downloading and Installing Fury from the Web

Fury can also be installed directly from the Internet with,
```sh Bash curl
curl -Ls get.fury.build | sh
```
```sh Bash wget
wget -qO- get.fury.build | sh
```

Steps have been taken to make this as safe as possible: the `get.fury.build` domain uses HTTPS and the domain
name is protected by DNSSEC. However, executing untrusted code directly from the web is, in general, risky. So
be aware of this general principle.

### Manual installation

It is also possible to install Fury manually, and versions other than the latest version can be installed this
way.

Simply download the binary version from the [downloads page](/downloads), make it executable, and then move it
to a directory on your `PATH`, for example,
```sh
curl -Ls https://fury.build/downloads/fury-0.8.4 > fury
chmod +x fury
sudo mv fury /usr/local/bin/
```

### Java

Fury also requires a Java Development Kit (JDK) in order to run. If there is already a suitable `java` command
on the `PATH`, then this will be used. Currently Fury requires JDK 11 or later.

If the installed version of Java is not recent enough, or if Fury cannot find a suitable JDK at all, then it
will install its own private version. By default, this will be the latest [Adoptium](https://adoptium.net/)
release for your operating system and processor architecture.

## Stopping Fury

The first time `fury` is run, it will launch a daemon process in the background, but for subsequent runs, it
will connect to the server through a lightweight client. This is why Fury will take a fraction of a second to
start the first time it is called, but will be near-instantaneous on subsequent calls. There is more
information available on how the [client/server operation](/help/client-server) works.


