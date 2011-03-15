AC_INIT(README)
AC_CANONICAL_SYSTEM
AM_INIT_AUTOMAKE(gupnp-sharp-1.0, 0.1)
AM_MAINTAINER_MODE

API_VERSION=1.0.0.0
AC_SUBST(API_VERSION)

MONO_REQUIRED_VERSION=1.0

AC_MSG_CHECKING([whether we're compiling from CVS'])
if test -f "$srcdir/.cvs_version" ; then
	from_cvs=yes
else
if test -f "$srcdir/.svn" ; then
	from_cvs=yes
else
	from_cvs=no
fi
fi

AC_MSG_RESULT($from_cvs)

AC_SUBST(PATH)
AC_SUBST(LD_LIBRARY_PATH)

dnl Find pkg-config
AC_PATH_PROG(PKG_CONFIG, pkg-config, no)
if test "x$PKG_CONFIG" = "xno"; then
        AC_MSG_ERROR([You need to install pkg-config])
fi

MONO_REQUIRED_VERSION=0.95
dnl The base dependencies are the depencies needed to compile and link against gupnp-sharp
PKG_CHECK_MODULES(BASE_DEPENDENCIES, gupnp-1.0 >= 0.15.1) 
PKG_CHECK_MODULES(MONO_DEPENDENCY, mono >= $MONO_REQUIRED_VERSION, has_mono=true, has_mono=false)

if test "x$has_mono" = "xtrue"; then
GENERATED_SOURCES=generated/*.cs
AC_PATH_PROG(RUNTIME, mono, no)
AC_PATH_PROG(CSC, mcs, no)
if test `uname -s` = "Darwin"; then
	LIB_PREFIX=
	LIB_SUFFIX=.dylib
else
	LIB_PREFIX=.so
	LIB_SUFFIX=
fi
else
AC_PATH_PROG(CSC, csc.exe, no)
if test x$CSC = "xno"; then
	AC_MSG_ERROR([You need to install either mono or .Net])
else
GENERATED_SOURCES=generated\\\\*.cs
RUNTIME=
LIB_PREFIX=
LIB_SUFFIX=.dylib
fi
fi

AC_SUBST(GENERATED_SOURCES)
AC_SUBST(LIB_PREFIX)
AC_SUBST(LIB_SUFFIX)


AC_SUBST(BASE_DEPENDENCIES_CFLAGS)
AC_SUBST(BASE_DEPENDENCIES_LIBS)
dnl Find C# dependencies

PKG_CHECK_MODULES(GLIB_SHARP, glib-sharp-2.0 >= 1.9.0)
AC_SUBST(GLIB_SHARP_CFLAGS)
AC_SUBST(GLIB_SHARP_LIBS)

PKG_CHECK_MODULES(GAPI, gapi-2.0 >= 1.9.0)

AC_PATH_PROG(GACUTIL, gacutil, no)
if test "x$GACUTIL" = "xno" ; then
        AC_MSG_ERROR([No gacutil tool found])
fi

winbuild=no
case "$host" in
       *-*-mingw*|*-*-cygwin*)
               winbuild=yes
               ;;
esac
AM_CONDITIONAL(WINBUILD, test x$winbuild = xyes)

AC_SUBST(GACUTIL)

AC_OUTPUT([
Makefile
gupnp/gupnp-sharp.dll.config
gupnp/Makefile
gupnp/AssemblyInfo.cs
gssdp/gssdp-sharp.dll.config
gssdp/Makefile
gssdp/AssemblyInfo.cs
gupnp-sharp-1.0.pc
])
