# Overview

Some features of Allay are inspired by the wonderful
[Regolith project](https://bedrock-oss.github.io/regolith/).

Each Allay project resembles one or more coherent packs. For example adding a new
block into the game requires both a behavior pack for the behavior of the block
and a resource pack for the textures. If two or more packs are not coherent they
should not belong to the same Allay project.

{{#webinclude https://raw.githubusercontent.com/allay-mc/.github/main/profile/README.md description}}

Allay is a command-line tool which is controlled from a terminal. We
expect that you know how to navigate between files in a terminal and how to
edit files.

```admonish tip
You can use an editor like [Visual Studio Code](https://code.visualstudio.com/)
which lets you use a terminal within the editor. This means you can easily
navigate between files and edit them in a comfortable way while using the terminal
to run simple commands like `allay build`.
```

```admonish warning
This project is in a work-in-progress status. Many features as well as links might
not work yet. Consider waiting for a stable release if you want to use this program.
```


## Philosophy

{{#webinclude https://raw.githubusercontent.com/allay-mc/.github/main/profile/README.md philosophy}}


## Glossary

To avoid misconception between interchangably used terms, Allay strictly defines
the following terms. They are used this way in the documentation and projects
related to Allay.

Term      | Meaning
----------|------------------------------------------------------------------------------------------------------------------------------------------
Add-on    | An add-on might be either a built behavior pack, resource pack, skin pack or world template.
Directory | Windows users might not be familar with this word. Throughout the documentation we will use this term which basically just means "folder".
Pack      | Same as an add-on but not the built version.


## Scripts

Scripts enable a powerful way of automating several tasks. The script `templating`
provides support for [ERB](https://github.com/ruby/erb) enabling a way of flow
control and generation.

```console
allay add templating
```

```erb,lang=mcfunction
<%# using erb %>
<% 10.times do |i| %>
scoreboard objectives add position<%= i %>
<% end %>
```

```toml,icon=gear,filepath=allay.toml
[scripts]
base-path = "scripts"
pre = [
  { run = "templating.rb", with = "ruby" }
]
```


## What's next?

There is much more to discover and we hope this tool makes your Minecraft developing
experience more confortable. You can continue reading through the documentation to
learn how to use Allay.
