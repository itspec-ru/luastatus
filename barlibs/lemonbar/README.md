This barlib talks with `lemonbar`.

It joins all non-empty strings returned by widgets by a separator, which defaults to ` | `.

Warning
===
`lemonbar` has a critical bug that will likely spoil your impression of using luastatus with it; its author does not seem to care. For details, please see:

https://github.com/LemonBoy/bar/pull/198

https://github.com/LemonBoy/bar/issues/107

https://github.com/shdown/luastatus/issues/12#issuecomment-277616761 and down the thread.

Please use `lemonbar` from https://github.com/shdown/bar/tree/buffer-bug-fix, or simply apply this patch: https://github.com/shdown/bar/commit/eb0e9e79b9a35c3093f6e2e90d9fbd175a2305ef.patch

Redirections and `luastatus-lemonbar-launcher`
===
`lemonbar` is not capable of creating a bidirectional pipe itself; instead, it requires all the data to be written to its stdin and read from its stdout.
That’s why the input/output file descriptors of this barlib must be manually redirected.

A launcher, `luastatus-lemonbar-launcher`, is shipped with it; it spawns `lemonbar` (or whatever is in `LEMONBAR` environment variable) connected to a bidirectional pipe and executes `luastatus` (or whatever is in `LUASTATUS` environment variable) with `-b lemonbar` (or whatever is in `LUASTATUS_LEMONBAR_BARLIB` environment variable), all the required `-B` options, and additional arguments passed by you.

Pass each `lemonbar` argument with `-p`, then pass `--`, then pass luastatus arguments, e.g.:
````bash
luastatus-lemonbar-launcher -p -B#111111 -p -f'Droid Sans Mono for Powerline:pixelsize=12:weight=Bold' -- -Bseparator=' ' widget1.lua widget2.lua
````

`cb` return value
===
A string with lemonbar markup, an array (that is, a table with numeric keys) of such strings (all non-empty elements of which are joined by the separator), or nil. Nil or an empty table is equivalent to an empty string.

Functions
===
* `escaped_str = luastatus.plugin.escape(str)`

  Escapes text for lemonbar markup.

`event` argument
===
A string with a name of the command.

Options
===
* `in_fd=<fd>`

  File descriptor to read `lemonbar` input from. Usually set by the launcher.

* `out_fd=<fd>`

  File descriptor to write to. Usually set by the launcher.

* `separator=<string>`

  Set the separator.
