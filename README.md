The Mongoose OS command line tool
=================================

## Installing on Windows

Download and run [pre-built mos.exe](https://mongoose-os.com/downloads/mos-release/win/mos.exe).

## Installing on Ubuntu Linux

Use PPA:

```bash
$ sudo add-apt-repository ppa:mongoose-os/mos
$ sudo apt-get update
$ sudo apt-get install mos
```

Note: to use the very latest version instead of the released one, the last
command should be `sudo apt-get install mos-latest`

## Installing on Arch Linux

Use PKGBUILD:

```bash
$ git clone https://github.com/mongoose-os/mos
$ cd mos/mos/archlinux_pkgbuild/mos-release/
$ makepkg
$ pacman -U ./mos-*.tar.xz
```

Note: to use the very latest version from the git repo, instead of the released
one, invoke `makepkg` from `mos-tool/mos/archlinux_pkgbuild/mos-latest`.

## Installing Mac OS

Use homebrew:

```bash
$ brew tap cesanta/mos
$ brew install mos
```

## Building manually

You will need:
 * Git
 * Go version 1.10 or later
 * GNU Make
 * Python 3
 * libftdi + headers
 * libusb 1.0 + headers
 * Docker - optional, only for building Windows binaries on Mac or Linux.

Commands to install all the build dependencies:
 * Ubuntu Linux: `sudo apt-get install build-essential git golang-go python3 libftdi-dev libusb-1.0-0-dev pkg-config`
 * Mac OS X (via [Homebrew](https://brew.sh/)): `brew install coreutils libftdi libusb-compat pkg-config`
 * Windows 10: `TODO`

Clone the repo (note: doesn't have to be in `GOPATH`):

```
$ git clone https://github.com/mongoose-os/mos
$ cd mos
```

Fetch dependencies (only needed for the first build):

```
$ make deps
```

Build the binary:

```
$ make
```

It will produce `mos/mos` (or `mos/mos.exe` on Windows).

## Tool Usage

To get a full list of flags and commands that mos supports, use the following command.
```
$ mos help --full
```

Look for a way to enable more verbose logging while using the mos command. This would be helpful when trying to debug.
```
$ 
```

Available options in the `build` command:
  - Normal flags for build command:
    - arch
    - platform
    - local
    - repo
    - clean
    - server
  - Advanceed flags for `mos build` command:
    - build-dry-run
    - build-params
    - build-target
    - module
    - lib
    - libs-update-interval
    - build-cmd-extra
    - cflags-extra
    - cxxflags-extra
    - lib-extra
    - save-build-stat
    - no-platform-check
    - prefer-prebuilt-libs
    - build-var
    - cdef
    - no-libs-update
    - skip-clean-libs
 
 ## Build Process flow
 `buildHandler()` defined in `cli/build.go` is accessible to `cli/main.go` without explicitly importing it.
 This is because Go makes all the variables, constants and functions available to the source files belonging
 to the same package. In this case both `cli/build.go` and `cli/main.go` belong to `main` package.
 
  - `cli/main.go`
    - `init()` Package init
    - `main()`
    - `getCommand()` Convert string to command struct
    - `run()` Run the associated handler function
  - `cli/build.go`
    - `init()` Package init