## configure.ac
## Written by Gary V. Vaughan, 2012

## Copyright (c) 2012-2016 Gary V. Vaughan

## This file is part of lcurses.
## See README for license.
## ------------------------------------------------------------------------

AC_INIT([lcurses], [8], [http://github.com/lcurses/lcurses/issues])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_MACRO_DIR([m4])

AS_BOX([Configuring AC_PACKAGE_TARNAME AC_PACKAGE_VERSION])
echo

AM_INIT_AUTOMAKE([-Wall subdir-objects])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

## Check for programs
AC_PROG_CC
AC_PROG_MKDIR_P
AC_PROG_EGREP
AC_PROG_SED


## ------------------------------------------------------------------------
## Compiler flags for POSIX 2008 or better compliance.
## ------------------------------------------------------------------------

AC_USE_SYSTEM_EXTENSIONS
AC_CANONICAL_HOST
case $host_os in
  *nto*|*qnx*)
    # QNX is only POSIX 2001, but _XOPEN_SOURCE turns off other
    # functions luaposix can bind.
    AC_SUBST([POSIX_EXTRA_CPPFLAGS], [-DLPOSIX_2001_COMPLIANT])
    ;;

  darwin*|openbsd*)
    # Setting _XOPEN_SOURCE here messes up header declarations, but
    # all functions luaposix can bind are available without it.
    AC_SUBST([POSIX_EXTRA_CPPFLAGS], [-DLPOSIX_2008_COMPLIANT])
    ;;

  *)
    # Enable POSIX 2008.  Please send a $host_os match if your host
    # is not compliant.
    AC_SUBST([POSIX_EXTRA_CPPFLAGS], [-D_XOPEN_SOURCE=700])
    ;;
esac

## required by automake 1.12.x, not available in 1.10.x
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])

LT_PREREQ([2.2.6])
LT_INIT([disable-static])

module=yes eval shrext=$shrext_cmds

AC_SUBST([objdir])
AC_SUBST([shrext])
AC_SUBST(LIBTOOL_DEPS)

## Check for header files
AC_HEADER_STDC

## Curses
AX_WITH_CURSES
AC_ARG_VAR(CURSES_LIB, [linker flags for curses library])

save_LIBS=$LIBS
LIBS="$CURSES_LIB $LIBS"
AC_CHECK_FUNCS([resizeterm])
LIBS=$save_LIBS

## Use system implementation, or bundled replacement?
AC_CHECK_FUNCS([strlcpy])

## Lua
AX_PROG_LUA(5.1, 5.4)
AX_LUA_HEADERS
AC_PATH_PROG([SPECL], [specl], [:])

## ldoc
AC_PATH_PROG([LDOC], [ldoc], [false])
AM_CONDITIONAL([HAVE_LDOC], [test false != "$LDOC"])

## Generate output files
AC_CONFIG_HEADER(config.h)
AC_CONFIG_FILES([Makefile build-aux/config.ld])
AC_OUTPUT
