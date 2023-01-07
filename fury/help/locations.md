Fury uses various locations to store data.

## Executable

The `fury` command is a single executable file containing everything that is necessary to start Fury. By
default, this will be installed at `~/.local/bin/fury`, unless Fury is installed by the `root` user, in
which case it will be installed to `/usr/local/bin/fury`.

Note that `~/.local/bin` is not always on the `$PATH` environment variable, so it may need to be explicitly
added.

## Shared files

When Fury compiles some code for the first time, it will create some data such as `class` files. These will be
stored in `$XDG_CACHE_HOME/fury` or in `~/.cache/fury` if the environment variable is not set. Cache files are
not essential to Fury, and can be safely deleted at any time except when Fury is building.

## Runtime files

The first time Fury runs, it starts a server in the background, and later runs just connect to the server
through named pipes. These will be created in `$XDG_RUNTIME_DIR/fury`, or `/run/user/<userid>/fury` if
`$XDG_RUNTIME_DIR` is not set.

These files are required for Fury to run, and deleting them will cause Fury to crash. If Fury is forcefully
terminated, for example with `kill -9`, then stale sockets may remain here. These should be deleted before
restarting Fury.

## Configuration

While project-specific configuration should go in the file called `fury` in a project's root directory, Fury
has various system-specific configuration options which you can set. The file, `$XDG_CONFIG_HOME/fury/config`
is a [CoDL](https://codl.dev/) file containing these configuration options. If `$XDG_CONFIG_HOME` is not set,
then `~/.config/fury/config` will be used. The file is not essential, and default configuration options will be
used if it does not exist.

## Logging

Fury will log various operations while it runs. Logs are stored in `/var/log/fury.log` if this file exists or
the `/var/log` directory is writable by the user who starts Fury. Live logs can be viewed with,
```sh
tail -f /var/log/fury.log
```

If this file does not exist, it probably means that Fury cannot create it because `/var/log` is not writable.
You can rectify this by running,
```sh
sudo touch /var/log/fury.log
sudo chown <userid> /var/fury/log
fury stop
```

## Environment variables

Note that environment variables are set on the first run of the `fury` command, so will be taken from the shell
where the server is invoked. Subsequent runs of `fury` in shells with different environment variables won't
affect these locations. However, you can just run `fury stop` then rerun it to use new environment variables.
