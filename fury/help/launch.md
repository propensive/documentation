## Executable buildfiles

A Fury buildfile, usually named `fury` and residing in the root directory of a project, contains the definition
of the build for that project in CoDL format, while also being a valid Bash script which
can be executed.

This is possible because, when interpreting the file as a Bash script, the build definitions can be skipped,
and when interpreting the file as CoDL data, the bash script and the lines which delimit the build
definitions are interpreted as text nodes which are effectively ignored when processing the file. Furthermore,
Bash and CoDL comments are written the same way, so are equally ignored however the file is read.

The essence of a Fury buildfile will look similar to this:
```codl
#!/bin/sh
:<< script
project apricot
  # Project definitions in here

##
    . <(echo tw/Gn7T8iEVw...ymJC4w7dTw== | base64 -d | gzip -d)
```

Let's look at each of these lines in turn.

The line `#!/bin/sh` is a "shebang" line, and specifies to the shell how the file should be interpreted. Despite
being slightly peculiar, this is a Bash script, and can be interpreted with the `sh` command. The "shebang" line
is treated as a comment in CoDL (though CoDL does insist that this kind of comment appears at the very beginning
of the file).

The second line, `: << script`, invokes the `:` shell command, known as the "no-op" command, and passes it (as
indicated by the `<<`) a "heredoc", that is, an inline document, which should be terminated by the text `script`
appearing by itself on one line. This effectively makes everything between `: << script` and `script` a
multiline comment from Bash's perspective.

However, when the document is interpreted as CoDL, the line `: << script` is interpreted as an `:` node with
parameters `<<` and `script`. This is permitted by the CodL Schema for Fury buildfiles, but does not affect the
meaning of the buildfile in any significant way, but the lines which follow it will be interpreted as valid
CoDL.

The `script` line terminates the multiline comment for Bash, so everything which follows will be processed as
shell script. While as CoDL, it starts a new `script` node, and the lines which follow will be treated as the
content of that node, since they are indented by four spaces. Like the `:` node, this content is not processed.
Bash is indifferent to the lines being indented.

The script itself is designed to be as small as possible, and fits into a couple of hundred bytes when gzipped
and BASE64-encoded. The script uses the `.` command, equivalent to `source` to read and execute the script from
standard input (specified inside the `<(...)` argument).
