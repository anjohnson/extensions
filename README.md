# EPICS Extensions Build Instructions

## Extensions Directory Structure

EPICS software is divided into `<top>` areas. Examples of `<top>` areas
are EPICS Base itself, EPICS extensions, and simple or complicated EPICS
IOC applications.

The EPICS extensions `<top>` area has the following directory structure:

```
  <top>/
    Makefile
    configure/
        Makefile
        os/
        ...
    src/
        Makefile
        <extension>/
            Makefile
            <subdirectories and source files>
        <extension>/
            Makefile
            <subdirectories and source files>
        ...
    include/
    bin/
    lib/
    html/
    ...
```

The `<top>` directory is usually named "extensions". The configure
directory contains build configuration files, a Makefile and the os
subdirectory which has operating-system specific build configuration
files. Each `<extension>` directory tree contains source files and one
or more Makefiles for building a related set of extension programs or
library modules. The include, bin, lib, html, and similar directories
are created during the build to hold the products of the build process.

## Setup

1. Download the extensionsTop.tar.gz file:

Unzip and untar the extensionTop.tar.gz distribution file. Use WinZip on
Windows systems.

This will create the extensions directory structure including all
configure and configure/os build configuration files, this README and
Makefiles in extensions/ and extensions/src/.

2. Download desired extensions:

Many extensions are available from WWW pages at various EPICS sites.

3. Unzip and untar extensions:

Unzip and untar the extension distribution files to the extensions/src
directory. Use WinZip on Windows systems.

## Build requirements

To build EPICS base and extensions you will need following items
installed. Some items can be obtained via links from the APS EPICS
software distribution web page at
    http://www.aps.anl.gov/epics/download/extensions/

You may not have to build extensions at your site. Pre-built extensions
executables for Win32 are available via the EPICS software distribution
web page.

* ANSI C compiler or GNU gcc compiler.

* EPICS base distribution, R3.14.12 or later, available from
    http://www.aps.anl.gov/epics/download/base/

* GNU make, V3.81 or later.

* Perl, version 5.8.1 or later.

## Additional software requirements:

* X11 is needed by the following extensions:
  alh, burt, medm, probe, stripTool

* Motif is needed by the following extensions:
  alh, burt, medm, probe, StripTool

* Xrtgraph is optional for the following extensions:
  medm

* Tk-tcl is needed by the following extensions:
  casr

* Java is needed by the following extensions:
  jca, caj, vdct

## Build configuration

### EPICS_HOST_ARCH

The environment variable EPICS_HOST_ARCH must be set appropriately for
the build host. The script startup/EpicsHostArch which is provided in
the EPICS Base distribution can be used to get a value for this.

### EPICS Base

EPICS Base must be configured and built prior to building any extension

### RELEASE file

The EPICS_BASE definition in extensions/configure/RELEASE must point to
the `<top>` of your built EPICS base.

### Files configure/CONFIG_SITE and configure/os/CONFIG_SITE.*

The extensions/configure directory and its usual contents must be
present, and the CONFIG_SITE files properly configured prior to building
an extension. Comments in these files describe the various build
settings they control.

## Build commands

### To build a single extension (e.g. extension xyz):

NOTE: Some extensions that use library routines may require other
extensions that build those libraries to have been built first.

#### Inside the epics/extensions/src/xyz directory

```
    make            - To build and install the extension.
    make install    - To build and install the extension.
    make clean      - To clean temporary object files. Clean will
                      remove ALL O.<hostarch> subdirectoriess.
    make archclean  - Removes O.<hostarch> dirs but not O.Common
    make rebuild    - To clean, build and install the extension.
```

#### Partial build commands

```
    make clean.<arch>   - Removes the O.<hostarch> and
                          O.Common directories.
    make install.<arch> - Builds and installs <arch> only.
```

#### Building a specific object file

To compile a specific source file e.g. alh.c, from the source dir's
`O.<arch>` directory invoke `make <target_filename>` e.g.

```
    vi alh.c
    cd O.solaris-sparc
    make alh.o
```

The above example instructs make to build only alh.o.

### To build all extensions present in your epics tree:

NOTE: To build all extensions present in the source tree using one make
command, the DIRS list definition in extensions/src/Makefile needs to
contain all your extension directory names. Ensure that the dependancy
extensions names for an extension precede that extension name.

#### Inside the `<top>` directory

```
    make                - To build and install all extensions.
    make clean          - To clean temporary object files. Cleaning
                          removes ALL O.<hostarch> subdirectories.
    make rebuild        - To clean, build, and install all extensions.
```

#### Uninstalling all extensions

```
    make uninstall      - Remove install directories for this hostarch.
    make realuninstall  - Removes ALL install dirs
    make distclean      - Same as realclean cvsclean realuninstall.
```

#### Listing all make target commands

```
    make help           - Prints a list of valid make targets
```

## Build notes

### Install locations

Executables are installed into `$(INSTALL_LOCATION)/bin/<arch>` and
libraries into `$(INSTALL_LOCATION)/lib/<arch>`. $(INSTALL_LOCATION)
defaults to `<top>` but may be changed in the configure/CONFIG_SITE
file.

### Header file dependancies

During a normal build (a "make" or "make install"), a file
containing header file dependancies is generated in the `O.<arch>`
directory for each object file compiled from C or C++ sources.
These files share the object file name with a `.d` suffix.

### Object files

Compiler output object files are stored in the created `O.<arch>`
directories. This allows multiple host architectures to be built
and maintained in the same tree structure at the same time.

