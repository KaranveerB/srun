# srun
A program to help manage scripts/alias with a flexible subcommand like
structure.

The primary motivation for this is to better manage my growing alias file where
I have multiple relates aliases with the same prefix (which is better grouped
under a subcommand) and a more convenient place to store cool one liners.

This program is in development and many features/changes may be made over time.

## Configuring
The program reads from `$XDG_CONFIG_HOME/srun/command.toml` for commands. This
is usually `~/.config/srun/command.toml`.

Each command can have the following keys
* `command`: (optional for subcommands) A string of the command to execute.
* `desc`: (optional) description of the command/subcommand.

Any other key is treated as the name of the command. Commands can be nested to
create a subcommand in command tree structure.

For example
```toml
[msg]
bid-farewell = { command = "echo bye", desc = "says bye" }

[msg.greet]
desc = "greets the user"
command = "srun msg greet kind"
casual = { command = "echo sup", desc = "says sup" }
kind = { command = "echo hi", desc = "says hi" }
```

You can then use the program as follows
```sh
> srun msg greet
hi
> srun msg greet kind
hi
> srun msg greet casual
sup
> srun msg bid-farewell
bye
> srun msg --help
usage: srun msg [command]
commands:
    bid-farewell: says bye
    greet: greets the user
> srun msg greet --help
usage: srun msg greet [command]
greets the user

commands:
    casual: says sup
    kind: says hi
```
