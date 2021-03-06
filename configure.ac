dnl Process this file with autoconf to produce a configure script.

AC_PREREQ([2.63])
AC_INIT([OpenModelica],[dev],[https://trac.openmodelica.org/OpenModelica],[openmodelica],[https://openmodelica.org])

AC_LANG([C])
AC_PROG_CC

m4_include([common/m4/pre-commit.m4])
m4_include([common/m4/ombuilddir.m4])
m4_include([common/m4/omhome.m4])
AC_SUBST(RPATH_QMAKE)
AC_SUBST(SHREXT)

AC_SUBST(OMC_TARGET)
AC_ARG_WITH(cppruntime,  [  --with-cppruntime       (build the optional cppruntime simulation libraries)],[OMC_TARGET="$withval"],[OMC_TARGET="no"])

AC_MSG_CHECKING([if cppruntime is requested])
if test "$OMC_TARGET" = "yes"; then
  OMC_TARGET=all-runtimeCPPinstall
  AC_MSG_RESULT([yes])
else
  OMC_TARGET=
  AC_MSG_RESULT([no])
fi

AC_SUBST(OMLIBRARY_TARGET)
AC_ARG_WITH(omlibrary,  [  --with-omlibrary=[core,all,no]       (which libraries to build; default is core; the libraries used by the testsuite)],[OMLIBRARY_TARGET="$withval"],[OMLIBRARY_TARGET="core"])

AC_MSG_CHECKING([for omlibrary target])
case "$OMLIBRARY_TARGET" in
  "yes"|"core")
    OMLIBRARY_TARGET="omlibrary-core"
    AC_MSG_RESULT([core])
  ;;
  "no")
    OMLIBRARY_TARGET=""
    AC_MSG_RESULT([none])
  ;;
  "all")
    OMLIBRARY_TARGET="omlibrary-all"
    AC_MSG_RESULT([all])
  ;;
  *)
    AC_MSG_ERROR("unknown omlibrary target $OMLIBRARY_TARGET")
  ;;
esac

if test ! -z "$OMLIBRARY_TARGET"; then
  AC_MSG_CHECKING([for libraries directory])
  if test -f libraries/Makefile; then
    AC_MSG_RESULT([yes])
  else
    AC_MSG_ERROR([no])
  fi
fi

AC_SUBST(MODELICA3D_TARGET)
AC_MSG_CHECKING([if the user wants Modelica3D])
MODELICA3D_TARGET="yes"
AC_ARG_ENABLE(modelica3d, [  --disable-modelica3d          Modelica3D library (dependencies checked by cmake later on; enabled by default)],[test "xno" = x"$enableval" && MODELICA3D_TARGET=""],[])
if test -z "$MODELICA3D_TARGET" ; then
AC_MSG_RESULT([no])
else
AC_MSG_RESULT([yes])
MODELICA3D_TARGET="omlibrary-modelica3d"
fi

AC_SUBST(ALL_TARGETS)
for dir_target in OMCompiler-omc OMPlot-omplot OMEdit-omedit OMNotebook-omnotebook OMOptim-omoptim OMShell-omshell; do
  dir=`echo $dir_target | cut -d- -f1`
  if test ! -f "$dir/configure.ac"; then
    AC_MSG_NOTICE("Subproject $dir does not exist")
  else
    if !(cd "$dir" && autoconf); then
      AC_MSG_ERROR("Failed to autoconf $dir")
    fi
    ALL_TARGETS="$ALL_TARGETS `echo $dir_target | cut -d- -f2-`"
  fi
done

AC_SUBST(ALL_TARGETS)
if test -f testsuite/Makefile; then
  ALL_TARGETS="$ALL_TARGETS testsuite-depends"
fi

AC_CONFIG_SUBDIRS([OMCompiler OMPlot OMEdit OMNotebook OMOptim OMShell])

AC_OUTPUT(Makefile)
