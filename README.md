# Viz - Combining Modal Editing with Structural Regular Expressions

**This is a fork of https://github.com/martanne/vis, meant to be more friendly to my needs.**

Please see https://github.com/martanne/vis for the overall goals of that editor.  This readme is mostly about specifics to this fork.

**Name:** I've renamed it `viz` instead of `vis`, because on macOS there is already a standard program `/usr/bin/vis` that "display non-printable characters in a visual format", and related vis functionality in the standard lib (`man 3 vis`), which has nothing to do with this editor.  It's confusing.  `viz` also fits with the "z" theme of the [Zython project](https://zython.org/), which is the motivation for this editor.

Build instructions
------------------

In order to build vis you will need a
[C99](http://www.open-std.org/jtc1/sc22/wg14/www/docs/n1256.pdf)
compiler, a [POSIX.1\-2008](http://pubs.opengroup.org/onlinepubs/9699919799/)
compatible environment as well as:

- [curses](https://en.wikipedia.org/wiki/Curses_(programming_library)) \(mandatory\)
- [Lua](http://www.lua.org/) &gt;= 5.2 \(mandatory\)
- [TRE](http://laurikari.net/tre/) \(optional for more memory efficient regex search\)

Assuming these dependencies are met, execute:

    $ ./configure && make && sudo make install

By default the `configure` script will try to auto detect support for
Lua using `pkg-config(1)`. See `configure --help` for a list of supported
options. You can also manually tweak the generated `config.mk` file.

**NOTE about other dependencies:** The upstream project https://github.com/martanne/vis depended on libtermkey \(for terminal support\) and [lpeg](http://www.inf.puc-rio.br/~roberto/lpeg/) for syntax highlighting.  Those libraries used to be under active development when vis started, but in 2022 they are both _**completely done**_.  libtermkey is actually officially deprecated, and lpeg hasn't had a single commit in years.  So instead we include copies of a snapshot of their code here, and replace their archaic build system with just integrating it with vis's standard build system.   Since lpeg requires lua, of course now lua is a mandatory dependency, whereas it used to be optional.    Editing code without syntax highlighting and the ability to customize the editor is not an option these days, so requiring lua is fine.

## Persistent Configuration

Here's an example of how to get going with configuring vis.  The default for vis is dark mode.  If you want light mode by default, make `~/.config/vis,` with a lua program to set the theme in a persistent way:

```sh
~/.config/vis$ cat visrc.lua 
require('vis')
vis.events.subscribe(vis.events.INIT, function()    
   vis:command('set theme light-16')
end)  
```

Documentation
-------------

Most of the upstream docs for `vis` should apply just fine \-\- End user documentation can be found in the
[`vis(1)` manual page](http://martanne.github.io/vis/man/vis.1.html)
and the [Wiki](https://github.com/martanne/vis/wiki). Read the
[FAQ](https://github.com/martanne/vis/wiki/FAQ) for common questions.
Learn about some differences compared to
[`sam(1)`](https://github.com/martanne/vis/wiki/Differences-from-Sam) and
[`vim(1)`](https://github.com/martanne/vis/wiki/Differences-from-Vi(m)),
respectively.

[C API](https://vis.readthedocs.io/) as well as [Lua API](http://martanne.github.io/vis/doc/)
documentation is also available.
