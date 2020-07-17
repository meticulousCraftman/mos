EBikeGo Mongoose OS command line tool
=================================

## Building the tool

### Install Prerequisites
You will need:
 * Git
 * Go version 1.10 or later
 * GNU Make
 * Python 3
 * libftdi + headers
 * libusb 1.0 + headers
 * Docker - optional, only for building Windows binaries on Mac or Linux.

Commands to install all the build dependencies on Ubuntu Linux (Any Debian distro):
```
 sudo apt-get install build-essential git golang-go python3 libftdi-dev libusb-1.0-0-dev pkg-config
```

### Building
Clone the repo (note: doesn't have to be in `GOPATH`):

```
$ git clone https://github.com/meticulousCraftman/mos
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

It will produce `mos` binary in the present working directory :) That's our tool!

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
    - `buildHandler()` This function prepares the `buildParams` struct object to store information 
    about the current build and then call the `doBuild()` function passing it the address to the 
    `buildParams`structure.
    - `doBuild()`