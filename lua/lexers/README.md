Lua LPeg lexers for vis
=======================

Viz reuses the [Lua](http://www.lua.org/) [LPeg](http://www.inf.puc-rio.br/~roberto/lpeg/)
based lexers from the [Scintillua](http://foicica.com/scintillua/) project
which is now part of the [Scintilla 3.x branch](https://foicica.com/hg/scintilla/file/tip/lexlua).  We include a copy of this code in the `lpeg/` subdirectory.

# Viz integration

Viz searches the lexers in the following locations \(check the end of the
`:help` output for the exact paths used by your binary\):

 * `$VIS_PATH/lexers`
 * `./lua/lexers` relative to the binary location (using `/proc/self/exe`)
 * `$XDG_CONFIG_HOME/vis/lexers` where `$XDG_CONFIG_HOME` refers to
   `$HOME/.config` if unset.
 * `/etc/vis/lexers`
 * `/usr/local/share/vis/lexers` or `/usr/share/vis/lexers` depending on
    the build configuration
 * `package.path` the standard Lua search path is queried for `lexers/$name`

At runtime a specific lexer can be loded by means of `:set syntax <name>`
where `<name>` corresponds to the filename without the `.lua` extension.

# Adding new lexers

To add a new lexer, start with the template quoted below or a lexer of a
similiar language. Read the
[lexer module documentation](http://foicica.com/scintillua/api.html#lexer).
The [LPeg](http://www.inf.puc-rio.br/~roberto/lpeg/) introduction might also
be useful.

For development purposes it is recommended to test the lexers from a lua
script as described in the
[Scintillua manual](http://foicica.com/scintillua/manual.html#Using.Scintillua.as.a.Lua.Library).

To enable auto syntax highlighting when opening a file you can associate your
new lexer with a set of file extensions by adding a corresponding entry into
the table found in [`plugins/filetype.lua`](../plugins/filetype.lua) file.

Changes to existing lexers should also be sent upstream for consideration.

A template for new lexers:

```lua
-- ? LPeg lexer.

local l = require('lexer')
local token, word_match = l.token, l.word_match
local P, R, S = lpeg.P, lpeg.R, lpeg.S

local M = {_NAME = '?'}

-- Whitespace.
local ws = token(l.WHITESPACE, l.space^1)

M._rules = {
  {'whitespace', ws},
}

M._tokenstyles = {

}

return M
```

# Color Themes

The `../themes` directory contains the color schemes. To set a different theme,   add a command like the following one to your `visrc.lua`:

```lua
require('vis')
vis.events.subscribe(vis.events.INIT, function()
   -- misc configuration
   vis:command("set theme solarized")
   -- ...
end)
```

