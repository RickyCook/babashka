A [babushka][1] like clone, written in bash.

## Installing

Clone this repo to the location of your choice, and add `babashka/bin` to your path.

```bash
git clone https://github.com/richo/babashka
echo "export PATH=$PWD/babashka/bin:\${PATH}" >>.bashrc
```

or if you don't have git

```bash
curl https://raw.githubusercontent.com/richo/babashka/master/bootstrap.sh | sh /dev/stdin
```

Or if you've done this before and you want your deps too:

```bash
curl https://raw.githubusercontent.com/richo/babashka/master/bootstrap.sh | sh /dev/stdin -u your_github_username
```

Or if for some reason you felt compelled to name your deps strangely:

```bash
curl https://raw.githubusercontent.com/richo/babashka/master/bootstrap.sh | sh /dev/stdin -u your_github_username -r strangely_named_repo
```

## Organising deps

`babashka` looks for dependencies by searching the `./deps/`, `./babashka-deps/` and `~/.babashka/deps` folders for files ending in `.bash` or `.sh`.

Project-specific deps are conventionally kept in `./deps/` and global deps are conventionally kept in `~/.babashka/deps`.

For example, `~/projects/myapp/deps/deploy.sh` might contain deployment scripts for an app called `myapp`, while `~/.babashka/deps/packages.sh` might contain deps which install packages you commonly need on new systems.

## Writing deps

Write deps with a similar form to their babushka counterparts:

```bash

# dep zsh_installed
zsh_installed() {
  function is_met() {
    which zsh
  }
  function meet() {
    sudo aptitude install zsh
  }
  process # Process line is important, you must include it.
}

# dep mysql_environment
mysql_environment() {
  requires "mysql_server"
  requires "mysql_client"
  # Don't need process if this dep doesn't have meet or is_met
}

mysql_server() {
  function is_met() {
    which mysqld
  }
  function meet() {
    sudo aptitude install mysql-server
  }
  process
}

mysql_client() {
  function is_met() {
    which mysql
  }
  function meet() {
    sudo aptitude install mysql-client
  }
  process
}
```

## Running deps

Then invoke:

```bash

babashka zsh_installed
babashka mysql_environment
```

## What people are saying about babashka

"You're a bad person"

~ [@larsyencken](https://twitter.com/larsyencken)

[1]: https://babushka.me
