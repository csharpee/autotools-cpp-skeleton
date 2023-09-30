# Our First Cpp Autotools Sandbox

## Requirements

* GCC
* GNU Autotools

## Steps to reproduce a Cpp Autotools Sandbox

1. Make root project directory e.g. /cpp-sandbox
2. Make project directories for organizing. e.g. ./build, ./test, ./m4, ./src
3. Make Makefile.am and edit it to your project needs.

```
# reduce warnings/errors
AUTOMAKE_OPTIONS = subdir-objects

# the name of the binary file
bin_PROGRAMS = sandbox

# various *.cpp and *.h files for the root project; the '\' implies a *newline*
sandbox_SOURCES = \
	src/main.cpp
```

4. *optional:* use `autoscan` command. Aautoscan creates a configure.log and configure.autoscan file that will need to be renamed to configure.ac too work with autotools formatting regulations. 
The autoscan utility creates a template that configure.ac can use for your project, rather than remembering and manually typing these features for your new GNU project.
5. then we let autoreconf to do the work: `autreconf -vi` Anytime you modify or create a configure.ac, you must reconfigure the autoconf system. -v = verbose -i = copy missing aux files. see `man autoreconf`
6. `cd build` and `../configure` This keeps autoconf files seperate from project files and creates a Makefile.
7. then, to compile the sandbox program we run `make` while in the **./build** dir. If we update our **main.cpp**, then we only need to re-run make, but if we update the configure.ac, then we must repeat step 5.
8. *optional:* to install the program into core operating system, we use `make install`. Prefixing must also be configured if needing a custom install location, see `man make` --prefix.
9. *optional:* cleanup and initialize the repository for git version management.

* Initialize git with `git init`
* Add a .gitignore file `touch .gitignore`
* Add the following to your .gitignore; this is a default we make use of for our GNU autotools cpp project.
```
# http://www.gnu.org/software/automake

Makefile.in
/ar-lib
/mdate-sh
/py-compile
/test-driver
/ylwrap
.deps/
.dirstamp

# http://www.gnu.org/software/autoconf

autom4te.cache
/autoscan.log
/autoscan-*.log
/aclocal.m4
/compile
/config.cache
/config.guess
/config.h.in
/config.log
/config.status
/config.sub
/configure
/configure~
/configure.scan
/depcomp
/install-sh
/missing
/stamp-h1

# https://www.gnu.org/software/libtool/

/ltmain.sh

# http://www.gnu.org/software/texinfo

/texinfo.tex

# http://www.gnu.org/software/m4/

m4/libtool.m4
m4/ltoptions.m4
m4/ltsugar.m4
m4/ltversion.m4
m4/lt~obsolete.m4

# exclude all build dir files

build/*

# exclude any *.o files in src dir

src/*.o

# exclude executables

/sandbox

# exclude any .env files in root project for securities sake

.env

# exclude any project files created from IDE

project.geany

# Generated Makefile
# (meta build system like autotools,
# can automatically generate from config.status script
# (which is called by configure script))
Makefile

```

## Cleanup Makefile Stuff

There are default Makefile commands being used, but more than likely, you will eventually want to *extend* the Automake Rules for cleaning up your project.
```
As the GNU Standards aren’t always explicit as to which files should be removed by which rule, we’ve adopted a heuristic that we believe was first formulated by François Pinard:

    If make built it, and it is commonly something that one would want to rebuild (for instance, a .o file), then mostlyclean should delete it.
    Otherwise, if make built it, then clean should delete it.
    If configure built it, then distclean should delete it.
    If the maintainer built it (for instance, a .info file), then maintainer-clean should delete it. However maintainer-clean should not delete anything that needs to exist in order to run ‘./configure && make’. 
```


### References: 

* Creating a Simple Autotools Based Project - Aaron Carlow https://www.youtube.com/watch?v=dc1kEJvS248

* Clean Automake: https://www.gnu.org/savannah-checkouts/gnu/automake/manual/html_node/Clean.html#Clean

* Git Ignore Documentation: https://git-scm.com/docs/gitignore

### Authors:

* oDinZu WenKi
