<p align="center">
  <img src="doc/logo-header.svg" alt="bat - a cat clone with wings"><br>
  <a href="https://travis-ci.org/sharkdp/bat"><img src="https://travis-ci.org/sharkdp/bat.svg?branch=master" alt="Build Status"></a>
  <a href="https://ci.appveyor.com/project/sharkdp/bat"><img src="https://ci.appveyor.com/api/projects/status/cptsmtbiwbnr2vhf/branch/master?svg=true"></a>
  <img src="https://img.shields.io/crates/l/bat.svg" alt="license">
  <a href="https://crates.io/crates/bat"><img src="https://img.shields.io/crates/v/bat.svg?colorB=319e8c" alt="Version info"></a><br>
  Клон утилиты <i>cat(1)</i> с поддержкой выделения синтаксиса и Git
</p>

<p align="center">
  <a href="#syntax-highlighting">Главные преимущества</a> •
  <a href="#how-to-use">Использование</a> •
  <a href="#installation">Установка</a> •
  <a href="#customization">Кастомизация</a> •
  <a href="#project-goals-and-alternatives">Цели проекта и его альтернативы </a> •
  Translation [<a href="https://github.com/chinanf-boy/bat-zh">中文</a>][<a href="doc/README-ja.md">日本語</a>][<a href="doc/README-ko.md">한국어</a>][<a href="doc/README-ru.md">日本語</a>][<a href="doc/README-ko.md">Русский</a>]
</p>

### Выделение синтаксиса

`bat` поддерживает выделение синтаксиса для огромного количества языков программирования и разметки:

![Пример выделения синтаксиса](https://i.imgur.com/3FGy5tW.png)

### Интеграция с Git
`bat` использует `git` чтобы показать изменения в коде
(смотрите на левый сайдбар):

![Пример интеграции с Git](https://i.imgur.com/azUAzdx.png)

### Show non-printable characters

Вы можете использовать `-A` / `--show-all` флаг чтобы показать символы, которые невозможно напечатать:

![Строка с неотображемыми символами](https://i.imgur.com/X0orYY9.png)

### Автоматическое разделение текста

`bat` умеет траслировать вывод в `less` если вывод слишком большой, чтобы уместится на экране целиком

### Объединение файлов

О..вы так же можете объединять файлы :wink:. Всегда когда
`bat` обнаруживает неинтерактивный терминал (например когда вы направляете вывод в файл или процесс), `bat` будет работать как вставная замена `cat` и откатится к выводу это файла как обычного текста

## Как использовать

Вывести единственный файл в терминале

```bash
> bat README.md
```

Отобразить сразу несколько файлов в терминале

```bash
> bat src/*.rs
```

Читаем из stdin и определяем синтаксис автоматически

```bash
> curl -s https://sh.rustup.rs | bat
```

Читает из stdin и указываем что это за язык

```bash
> yaml2json .travis.yml | json_pp | bat -l json
```

Вывести и выделить неотображаемые символы
```bash
> bat -A /etc/hosts
```

Использование как замена `cat`

```bash
bat > note.md  # мгновенно создаем новый файл

bat header.md content.md footer.md > document.md

bat -n main.rs  # показываем только количество строк

bat f - g  # выводит 'f' в stdin, а потом 'g'.
```

### Интеграция с другими утилитами

#### `find` или `fd`

Вы можете использовать флаг `-exec` в `find` чтобы посмотреть превью всех файлов в `bat`
```bash
find … -exec bat {} +
```

Если вы используете [`fd`](https://github.com/sharkdp/fd), вы вполне можете использоваль флаг  `-X`/`--exec-batch` чтобы сделать тоже самое:
```bash
fd … -X bat
```

#### `ripgrep`

С помощью [`batgrep`](https://github.com/eth-p/bat-extras/blob/master/doc/batgrep.md), `bat` может быть использован для вывода результата запроса [`ripgrep`](https://github.com/BurntSushi/ripgrep)

```bash
batgrep needle src/
```

#### `tail -f`

`bat` can be combined with `tail -f` to continuously monitor a given file with syntax highlighting.
```bash
tail -f /var/log/pacman.log | bat --paging=never -l log
```
Note that we have to switch off paging in order for this to work. We have also specified the syntax
explicitly (`-l log`), as it can not be auto-detected in this case.

#### `git`

You can combine `bat` with `git show` to view an older version of a given file with proper syntax
highlighting:
```bash
git show v0.6.0:src/main.rs | bat -l rs
```

Note that syntax highlighting within diffs is currently not supported. If you are looking for this, check out [`delta`](https://github.com/dandavison/delta).

#### `xclip`

The line numbers and Git modification markers in the output of `bat` can make it hard to copy
the contents of a file. To prevent this, you can call `bat` with the `-p`/`--plain` option or
simply pipe the output into `xclip`:
```bash
bat main.cpp | xclip
```
`bat` will detect that the output is being redirected and print the plain file contents.

#### `man`

`bat` can be used as a colorizing pager for `man`, by setting the
`MANPAGER` environment variable:

```bash
export MANPAGER="sh -c 'col -bx | bat -l man -p'"
man 2 select
```

It might also be necessary to set `MANROFFOPT="-c"` if you experience
formatting problems.

If you prefer to have this bundled in a new command, you can also use [`batman`](https://github.com/eth-p/bat-extras/blob/master/doc/batman.md).

Note that the [Manpage syntax](assets/syntaxes/02_Extra/Manpage.sublime-syntax) is developed in this repository and still needs some work.

#### `prettier` / `shfmt` / `rustfmt`

The [`prettybat`](https://github.com/eth-p/bat-extras/blob/master/doc/prettybat.md) script is a wrapper that will format code and print it with `bat`.


## Installation

[![Packaging status](https://repology.org/badge/vertical-allrepos/bat.svg)](https://repology.org/project/bat/versions)

### On Ubuntu (using `apt`)
*... and other Debian-based Linux distributions.*

`bat` is making its way through the [Ubuntu](https://packages.ubuntu.com/eoan/bat) and
[Debian](https://packages.debian.org/sid/bat) package release process, and is available
for Ubuntu as of Eoan 19.10. On Debian `bat` is currently only available on the unstable
"Sid" branch.

If your Ubuntu/Debian installation is new enough you can simply run:

```bash
apt install bat
```

If you install `bat` this way, please note that the executable may be installed as `batcat` instead of `bat` (due to [a name
clash with another package](https://github.com/sharkdp/bat/issues/982)). You can set up a `bat -> batcat` symlink or alias to prevent any issues that may come up because of this and to be consistent with other distrutions:
``` bash
mkdir -p ~/.local/bin
ln -s /usr/bin/batcat ~/.local/bin/bat
```

### On Ubuntu (using most recent `.deb` packages)
*... and other Debian-based Linux distributions.*

If the package has not yet been promoted to your Ubuntu/Debian installation, or you want
the most recent release of `bat`, download the latest `.deb` package from the
[release page](https://github.com/sharkdp/bat/releases) and install it via:

```bash
sudo dpkg -i bat_0.15.4_amd64.deb  # adapt version number and architecture
```

### On Alpine Linux

You can install [the `bat` package](https://pkgs.alpinelinux.org/packages?name=bat)
from the official sources, provided you have the appropriate repository enabled:

```bash
apk add bat
```

### On Arch Linux

You can install [the `bat` package](https://www.archlinux.org/packages/community/x86_64/bat/)
from the official sources:

```bash
pacman -S bat
```

### On Fedora

You can install [the `bat` package](https://koji.fedoraproject.org/koji/packageinfo?packageID=27506) from the official [Fedora Modular](https://docs.fedoraproject.org/en-US/modularity/using-modules/) repository.

```bash
dnf install bat
```

### On Gentoo Linux

You can install [the `bat` package](https://packages.gentoo.org/packages/sys-apps/bat)
from the official sources:

```bash
emerge sys-apps/bat
```

### On Void Linux

You can install `bat` via xbps-install:
```bash
xbps-install -S bat
```

### On FreeBSD

You can install a precompiled [`bat` package](https://www.freshports.org/textproc/bat) with pkg:

```bash
pkg install bat
```

or build it on your own from the FreeBSD ports:

```bash
cd /usr/ports/textproc/bat
make install
```

### Via nix

You can install `bat` using the [nix package manager](https://nixos.org/nix):

```bash
nix-env -i bat
```

### On openSUSE

You can install `bat` with zypper:

```bash
zypper install bat
```

### On macOS

You can install `bat` with [Homebrew](http://braumeister.org/formula/bat):

```bash
brew install bat
```

Or install `bat` with [MacPorts](https://ports.macports.org/port/bat/summary):

```bash
port install bat
```

### On Windows

There are a few options to install `bat` on Windows. Once you have installed `bat`,
take a look at the ["Using `bat` on Windows"](#using-bat-on-windows) section.

#### With Chocolatey

You can install `bat` via [Chocolatey](https://chocolatey.org/packages/Bat):
```bash
choco install bat
```

#### With Scoop

You can install `bat` via [scoop](https://scoop.sh/):
```bash
scoop install bat
```

You will need to install the [Visual C++ Redistributable](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) package.

#### From prebuilt binaries:

You can download prebuilt binaries from the [Release page](https://github.com/sharkdp/bat/releases),

You will need to install the [Visual C++ Redistributable](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) package.

### Via Docker

There is a [Docker image](https://hub.docker.com/r/danlynn/bat/) that you can use to run `bat` in a container:
```bash
docker pull danlynn/bat
alias bat='docker run -it --rm -e BAT_THEME -e BAT_STYLE -e BAT_TABS -v "$(pwd):/myapp" danlynn/bat'
```

### Via Ansible

You can install `bat` with [Ansible](https://www.ansible.com/):

```bash
# Install role on local machine
ansible-galaxy install aeimer.install_bat
```

```yaml
---
# Playbook to install bat
- host: all
  roles:
    - aeimer.install_bat
```

- [Ansible Galaxy](https://galaxy.ansible.com/aeimer/install_bat)
- [GitHub](https://github.com/aeimer/ansible-install-bat)

This should work with the following distributions:
- Debian/Ubuntu
- ARM (eg. Raspberry PI)
- Arch Linux
- Void Linux
- FreeBSD
- MacOS

### From binaries

Check out the [Release page](https://github.com/sharkdp/bat/releases) for
prebuilt versions of `bat` for many different architectures. Statically-linked
binaries are also available: look for archives with `musl` in the file name.

### From source

If you want to build `bat` from source, you need Rust 1.40 or
higher. You can then use `cargo` to build everything:

```bash
cargo install --locked bat
```

## Customization

### Highlighting theme

Use `bat --list-themes` to get a list of all available themes for syntax
highlighting. To select the `TwoDark` theme, call `bat` with the
`--theme=TwoDark` option or set the `BAT_THEME` environment variable to
`TwoDark`. Use `export BAT_THEME="TwoDark"` in your shell's startup file to
make the change permanent. Alternatively, use `bat`s
[configuration file](https://github.com/sharkdp/bat#configuration-file).

If you want to preview the different themes on a custom file, you can use
the following command (you need [`fzf`](https://github.com/junegunn/fzf) for this):
```bash
bat --list-themes | fzf --preview="bat --theme={} --color=always /path/to/file"
```

`bat` looks good on a dark background by default. However, if your terminal uses a
light background, some themes like `GitHub` or `OneHalfLight` will work better for you.
You can also use a custom theme by following the
['Adding new themes' section below](https://github.com/sharkdp/bat#adding-new-themes).

### Output style

You can use the `--style` option to control the appearance of `bat`s output.
You can use `--style=numbers,changes`, for example, to show only Git changes
and line numbers but no grid and no file header. Set the `BAT_STYLE` environment
variable to make these changes permanent or use `bat`s
[configuration file](https://github.com/sharkdp/bat#configuration-file).

### Adding new syntaxes / language definitions

`bat` uses the excellent [`syntect`](https://github.com/trishume/syntect/)
library for syntax highlighting. `syntect` can read any
[Sublime Text `.sublime-syntax` file](https://www.sublimetext.com/docs/3/syntax.html)
and theme. To add new syntax definitions, do the following.

Create a folder with syntax definition files:

```bash
mkdir -p "$(bat --config-dir)/syntaxes"
cd "$(bat --config-dir)/syntaxes"

# Put new '.sublime-syntax' language definition files
# in this folder (or its subdirectories), for example:
git clone https://github.com/tellnobody1/sublime-purescript-syntax
```

Now use the following command to parse these files into a binary cache:

```bash
bat cache --build
```

Finally, use `bat --list-languages` to check if the new languages are available.

If you ever want to go back to the default settings, call:

```bash
bat cache --clear
```

### Adding new themes

This works very similar to how we add new syntax definitions.

First, create a folder with the new syntax highlighting themes:
```bash
mkdir -p "$(bat --config-dir)/themes"
cd "$(bat --config-dir)/themes"

# Download a theme in '.tmTheme' format, for example:
git clone https://github.com/greggb/sublime-snazzy

# Update the binary cache
bat cache --build
```

Finally, use `bat --list-themes` to check if the new themes are available.

### Using a different pager

`bat` uses the pager that is specified in the `PAGER` environment variable. If this variable is not
set, `less` is used by default. If you want to use a different pager, you can either modify the
`PAGER` variable or set the `BAT_PAGER` environment variable to override what is specified in
`PAGER`.

If you want to pass command-line arguments to the pager, you can also set them via the
`PAGER`/`BAT_PAGER` variables:

```bash
export BAT_PAGER="less -RF"
```

Instead of using environment variables, you can also use `bat`s [configuration file](https://github.com/sharkdp/bat#configuration-file) to configure the pager (`--pager` option).

**Note**: By default, if the pager is set to `less` (and no command-line options are specified),
`bat` will pass the following command line options to the pager: `-R`/`--RAW-CONTROL-CHARS`,
`-F`/`--quit-if-one-screen` and `-X`/`--no-init`. The last option (`-X`) is only used for `less`
versions older than 530.

The `-R` option is needed to interpret ANSI colors correctly. The second option (`-F`) instructs
less to exit immediately if the output size is smaller than the vertical size of the terminal.
This is convenient for small files because you do not have to press `q` to quit the pager. The
third option (`-X`) is needed to fix a bug with the `--quit-if-one-screen` feature in old versions
of `less`. Unfortunately, it also breaks mouse-wheel support in `less`.

If you want to enable mouse-wheel scrolling on older versions of `less`, you can pass just `-R` (as
in the example above, this will disable the quit-if-one-screen feature). For less 530 or newer,
it should work out of the box.

### Dark mode

If you make use of the dark mode feature in macOS, you might want to configure `bat` to use a different
theme based on the OS theme. The following snippet uses the `default` theme when in the _dark mode_
and the `GitHub` theme when in the _light mode_.

```bash
alias cat="bat --theme=\$(defaults read -globalDomain AppleInterfaceStyle &> /dev/null && echo default || echo GitHub)"
```

## Configuration file

`bat` can also be customized with a configuration file. The location of the file is dependent
on your operating system. To get the default path for your system, call
```
bat --config-file
```

Alternatively, you can use the `BAT_CONFIG_PATH` environment variable to point `bat` to a
non-default location of the configuration file:
```bash
export BAT_CONFIG_PATH="/path/to/bat.conf"
```

A default configuration file can be created with the `--generate-config-file` option.
```bash
bat --generate-config-file
```

### Format

The configuration file is a simple list of command line arguments. Use `bat --help` to see a full list of possible options and values. In addition, you can add comments by prepending a line with the `#` character.

Example configuration file:
```bash
# Set the theme to "TwoDark"
--theme="TwoDark"

# Show line numbers, Git modifications and file header (but no grid)
--style="numbers,changes,header"

# Use italic text on the terminal (not supported on all terminals)
--italic-text=always

# Use C++ syntax for .ino files
--map-syntax "*.ino:C++"

# Use ".gitignore"-style highlighting for ".ignore" files
--map-syntax ".ignore:Git Ignore"
```

## Using `bat` on Windows

`bat` mostly works out-of-the-box on Windows, but a few features may need extra configuration.

### Paging

Windows only includes a very limited pager in the form of `more`. You can download a Windows binary
for `less` [from its homepage](http://www.greenwoodsoftware.com/less/download.html) or [through
Chocolatey](https://chocolatey.org/packages/Less). To use it, place the binary in a directory in
your `PATH` or [define an environment variable](#using-a-different-pager). The [Chocolatey package](#on-windows) installs `less` automatically.

### Colors

Windows 10 natively supports colors in both `conhost.exe` (Command Prompt) and PowerShell since
[v1511](https://en.wikipedia.org/wiki/Windows_10_version_history#Version_1511_(November_Update)), as
well as in newer versions of bash. On earlier versions of Windows, you can use
[Cmder](http://cmder.net/), which includes [ConEmu](https://conemu.github.io/).

**Note:** The Git and MSYS versions of `less` do not correctly interpret colors on Windows. If you
don’t have any other pagers installed, you can disable paging entirely by passing `--paging=never`
or by setting `BAT_PAGER` to an empty string.

### Cygwin

`bat` on Windows does not natively support Cygwin's unix-style paths (`/cygdrive/*`). When passed an absolute cygwin path as an argument, `bat` will encounter the following error: `The system cannot find the path specified. (os error 3)`

This can be solved by creating a wrapper or adding the following function to your `.bash_profile` file:

```bash
bat() {
    local index
    local args=("$@")
    for index in $(seq 0 ${#args[@]}) ; do
        case "${args[index]}" in
        -*) continue;;
        *)  [ -e "${args[index]}" ] && args[index]="$(cygpath --windows "${args[index]}")";;
        esac
    done
    command bat "${args[@]}"
}
```

## Troubleshooting

### Terminals & colors

`bat` handles terminals *with* and *without* truecolor support. However, the colors in the syntax
highlighting themes are not optimized for 8-bit colors and it is therefore strongly recommended
that you use a terminal with 24-bit truecolor support (`terminator`, `konsole`, `iTerm2`, ...).
See [this article](https://gist.github.com/XVilka/8346728) for more details and a full list of
terminals with truecolor support.

Make sure that your truecolor terminal sets the `COLORTERM` variable to either `truecolor` or
`24bit`. Otherwise, `bat` will not be able to determine whether or not 24-bit escape sequences
are supported (and fall back to 8-bit colors).

### Line numbers and grid are hardly visible

Please try a different theme (see `bat --list-themes` for a list). The `OneHalfDark` and
`OneHalfLight` themes provide grid and line colors that are brighter.

### File encodings

`bat` natively supports UTF-8 as well as UTF-16. For every other file encoding, you may need to
convert to UTF-8 first because the encodings can typically not be auto-detected. You can `iconv`
to do so.
Example: if you have a PHP file in Latin-1 (ISO-8859-1) encoding, you can call:
``` bash
iconv -f ISO-8859-1 -t UTF-8 my-file.php | bat
```
Note: you might have to use the `-l`/`--language` option if the syntax can not be auto-detected
by `bat`.

## Development

```bash
# Recursive clone to retrieve all submodules
git clone --recursive https://github.com/sharkdp/bat

# Build (debug version)
cd bat
cargo build --bins

# Run unit tests and integration tests
cargo test

# Install (release version)
cargo install --locked

# Build a bat binary with modified syntaxes and themes
bash assets/create.sh
cargo install --locked --force
```

## Maintainers

- [sharkdp](https://github.com/sharkdp)
- [eth-p](https://github.com/eth-p)

## Project goals and alternatives

`bat` tries to achieve the following goals:

- Provide beautiful, advanced syntax highlighting
- Integrate with Git to show file modifications
- Be a drop-in replacement for (POSIX) `cat`
- Offer a user-friendly command-line interface

There are a lot of alternatives, if you are looking for similar programs. See
[this document](doc/alternatives.md) for a comparison.

## License
Copyright (c) 2018-2020 [bat-developers](https://github.com/sharkdp/bat).

`bat` is distributed under the terms of both the MIT License and the Apache License 2.0.

See the [LICENSE-APACHE](LICENSE-APACHE) and [LICENSE-MIT](LICENSE-MIT) files for license details.