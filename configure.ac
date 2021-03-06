dnl Process this file with autoconf to produce a configure script.
AC_INIT(fenice, 1.11)
AC_CONFIG_AUX_DIR(config)
AC_CANONICAL_SYSTEM
AM_INIT_AUTOMAKE(dist-bzip2 check-news)

AC_PREFIX_PROGRAM(fenice)

test "x$prefix" = x. || test "x$prefix" = xNONE && prefix=/usr/local
test "x$exec_prefix" = xNONE && exec_prefix=$prefix
test "x$sysconfdir" = 'x${prefix}/etc' && sysconfdir="/etc"
test "x$localstatedir" = 'x${prefix}/var' && localstatedir="/var"
test "x$mandir" = 'x${prefix}/man' && mandir="${prefix}/share/man"

AM_CONFIG_HEADER(config.h)

AC_ARG_WITH(rtsp-port,
[  --with-rtsp-port        default RTSP listening port [[default=554, alternative=8554]]],,
	with_rtsp_port=554)

AC_ARG_WITH(config-file,
[  --with-config-file      name for the configuration file in sysconfdir [[default=fenice.conf]]],,
	with_config_file="fenice.conf")

AC_ARG_WITH(log-file,
[  --with-log-file	   name for the log file [[default=/var/log/fenice.log]]],,
	with_log_file="/var/log/fenice.log")

AC_ARG_WITH(max-session,
[  --with-max-session      max number of incoming sessions [[default=100]]],,
	with_max_session=100)

AC_ARG_ENABLE(fhs23,
[  --enable-fhs23          enable Filesystem Hierarchy Standard 2.3:
                          document root avroot will be placed under /srv [[default=yes]]],,
	enable_fhs23="yes")

AC_ARG_ENABLE(debug,
[  --enable-debug          enable gcc dubugging flags [[default=no]]],,
	enable_debug="no")

AC_ARG_ENABLE(efence,
[  --enable-efence         link against Electric Fence library to debug memory usage
                          (this will involve --enable-debug) [[default=no]]],,
	enable_efence="no")


AC_ARG_ENABLE(verbose,
[  --enable-verbose          make fenice verbose [[default=no]]],,
	enable_verbose="no")

AC_ARG_ENABLE(dump,
[  --enable-dump          enable dump 
			  (this will involve --enable-verbose) [[default=no]]],,
	enable_dump="no")

AC_ARG_ENABLE(dumpbuff,
[  --enable-dumpbuff       enable bufferpool dump 
			  (this will involve --enable-verbose) [[default=no]]],,
	enable_dumpbuff="no")
AC_ARG_ENABLE(warnings,
[  --enable-warnings       enable/choose gcc warnings level: none, normal, pedantic [[default=none]]],,
	enable_warnings="none")
AC_ARG_ENABLE(errors,
[  --enable-errors         make gcc warnings behave like errors: none, normal, pedantic [[default=none]]],,
	enable_errors="none")
AC_ARG_ENABLE(optimize,
[  --enable-optimize       enable/chose optimization level: none, poor, normal, full, super [[default=normal]]],,
	enable_optimize="normal")

AC_PROG_CC
AC_PROG_GCC_TRADITIONAL
AC_PROG_CPP
AC_PROG_AWK
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET

dnl Check for LibTool Dynamic Loading
AC_PROG_LIBTOOL
LIBTOOL="$LIBTOOL --silent"
AC_SUBST([LIBTOOL_DEPS])

dnl Check for Endianess
AC_C_BIGENDIAN

dnl Checks for libraries.
AC_CHECK_LIB(pthread,pthread_create)
dnl AC_CHECK_LIB(avcodec,avcodec_open)

dnl Replace `main' with a function in -lm:
AC_CHECK_LIB(m, main)

dnl Checks for header files.
AC_HEADER_DIRENT
AC_HEADER_STDC
AC_HEADER_TIME
AC_CHECK_HEADERS(sys/time.h unistd.h strings.h errno.h fcntl.h malloc.h sys/ioctl.h getopt.h syslog.h) 

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_TYPE_SIZE_T
AC_TYPE_UID_T
AC_TYPE_PID_T
AC_C_INLINE
AC_HEADER_TIME

dnl Checks for library functions.
AC_FUNC_ALLOCA
AC_FUNC_MEMCMP
AC_FUNC_MMAP
AC_FUNC_VPRINTF
AC_CHECK_FUNCS(select socket gettimeofday uname getcwd getwd strcspn strdup strtoul strerror strstr setenv)
AC_CHECK_FUNC(getaddrinfo)

dnl check if we have the generic struct for net addresses the has max possible size
AC_CHECK_TYPES(struct sockaddr_storage,,, [#include <sys/socket.h>])

CFLAGS=""


AM_CONDITIONAL(FENICE_EFENCE, /bin/false)
if test "$enable_efence" = "yes"; then
	AM_CONDITIONAL(FENICE_EFENCE, /bin/true)
	AC_DEFINE(ENABLE_EFENCE, 1,[Debug enabled])
	enable_debug=yes
	efence_msg="   You chose to compile against Electric Fence lib"
fi

AM_CONDITIONAL(FENICE_DEBUG, /bin/false)
if test "$enable_debug" = "yes"; then
	CFLAGS="$CFLAGS -g -ggdb"
	AM_CONDITIONAL(FENICE_DEBUG, /bin/true)
	AC_DEFINE(ENABLE_DEBUG, 1,[Debug enabled])
	debug_msg="
   Debuging flags enabled: welcome developer!!!"
else
	AC_DEFINE(ENABLE_DEBUG, 0,[Debug disabled])
fi

AM_CONDITIONAL(FENICE_DUMP, /bin/false)
if test "$enable_dump" = "yes"; then
	AM_CONDITIONAL(FENICE_DUMP, /bin/true)
	AC_DEFINE(ENABLE_DUMP, 1,[Dump enabled])
	enable_verbose=yes
	dump_msg="
   Dump to dump_fenice.ENCODING_TYPE.rtpsock enabled!!!"
dnl else
dnl	AC_DEFINE(ENABLE_DUMP, 0,[Dump disabled])
fi

AM_CONDITIONAL(FENICE_DUMPBUFF, /bin/false)
if test "$enable_dumpbuff" = "yes"; then
	AM_CONDITIONAL(FENICE_DUMPBUFF, /bin/true)
	AC_DEFINE(ENABLE_DUMPBUFF, 1, [Buffer Dump enabled])
	enable_verbose=yes
	dumpbuff_msg="
   Dump of bufferpool is enabled!!!"
else
	AC_DEFINE(ENABLE_DUMPBUFF, 0,[Buffer Dump disabled])
fi

AM_CONDITIONAL(FENICE_VERBOSE, /bin/false)
if test "$enable_verbose" = "yes"; then
	AM_CONDITIONAL(FENICE_VERBOSE, /bin/true)
	AC_DEFINE(ENABLE_VERBOSE, 1,[verbosity enabled])
	enable_verbose=yes
	verbose_msg="
   You chose fenice to be very verbose... who'll stop it talking now!!!"
fi




case "$enable_warnings" in
	pedantic)
		CFLAGS="$CFLAGS -W -Wall -pedantic"
	;;
	normal | yes)
		CFLAGS="$CFLAGS -W -Wall"
	;;
	none | *)
		CFLAGS="$CFLAGS"
	;;
esac

case "$enable_errors" in
	pedantic)
		CFLAGS="$CFLAGS -pedantic-errors -Werror"
	;;
	normal | yes)
		CFLAGS="$CFLAGS -Werror"
	;;
	none | *)
		CFLAGS="$CFLAGS"
	;;
esac

case "$enable_optimize" in
	poor)
		CFLAGS="$CFLAGS -O1" 
	;;
	full)
		CFLAGS="$CFLAGS -O3"
	;;
	super)
		CFLAGS="$CFLAGS -O3 -funroll-loops -fstrength-reduce -fomit-frame-pointer -ffast-math -malign-functions=2 -malign-jumps=2 -malign-loops=2"
	;;
	none)
		CFLAGS="$CFLAGS"
	;;
	normal | *)
		CFLAGS="$CFLAGS -O2"
	;;
esac


ARCH_DEFINES=""
case "$host" in 
	i386-*-* | i486-*-* | i586-*-* | i686-*-* | i86pc-*-*)
		CFLAGS="$CFLAGS -march=i386"
		ARCH_DEFINES="-DI386_ASSEM"
		arch_type=ix86
	;;
	ppc-*-*)
		arch_type=ppc
	;;
	*-hpux-*)
		ARCH_DEFINES="-DHPUX"
	;;
	*-sun-*)
		have_solaris=yes
	;;
esac


AC_SUBST(ARCH_DEFINES)
AM_CONDITIONAL(ARCH_X86, test "x$arch_type" = "xix86")
AM_CONDITIONAL(ARCH_PPC, test "x$arch_type" = "xppc")
AM_CONDITIONAL(HAVE_SOLARIS, test "x$have_solaris" = xyes)

if ( test "x$enable_fhs23" = "xyes" ) && ( test -d /srv ) then
	fenice_avroot_dir=/srv/$PACKAGE_NAME/avroot
else
	fenice_avroot_dir=$localstatedir/$PACKAGE_NAME/avroot
fi
AC_SUBST(fenice_avroot_dir)

fenice_max_session=$with_max_session
AC_SUBST(fenice_max_session)

fenice_rtsp_port=$with_rtsp_port
AC_SUBST(fenice_rtsp_port)

fenice_conf_file=$with_config_file
AC_SUBST(fenice_conf_file)

fenice_log_file=$with_log_file
AC_SUBST(fenice_log_file)

fenice_conf_dir=$sysconfdir
AC_SUBST(fenice_conf_dir)

fenice_docs_dir=$datadir/doc/$PACKAGE_NAME
AC_SUBST(fenice_docs_dir)

if test -f svnrev.h; then
	AC_DEFINE(FENICE_SVN, 1,[Define if this is a development (subversion) tree])
fi

AC_DEFINE_UNQUOTED(FENICE_MAX_SESSION_DEFAULT, $fenice_max_session, [Define max number of RTSP incoming sessions for Fenice])

AC_DEFINE_UNQUOTED(FENICE_RTSP_PORT_DEFAULT, $fenice_rtsp_port, [Define default RTSP listening port])
dnl AC_DEFINE(FENICE_RTSP_PORT_ENV, "FENICE_RTSP_PORT",[Define the name for the ENV variable for RTSP listening port])

AC_DEFINE_UNQUOTED(FENICE_AVROOT_DIR_DEFAULT, $fenice_avroot_dir, [Define default directory for Fenice A/V resources])
dnl AC_DEFINE(FENICE_AVROOT_DIR_ENV, "FENICE_AVROOT_DIR",[Define the name for the ENV variable for Fenice avroot dir])

AC_DEFINE_UNQUOTED(FENICE_AVROOT_DIR_DEFAULT_STR, "$fenice_avroot_dir", [Define default directory string for Fenice A/V resources])
dnl AC_DEFINE(FENICE_AVROOT_DIR_STR_ENV, "FENICE_AVROOT_DIR",[Define the name for the ENV variable for Fenice avroot dir string])

AC_DEFINE_UNQUOTED(FENICE_CONF_FILE_DEFAULT, $fenice_conf_file, [Define default file for Fenice configuration])
dnl AC_DEFINE(FENICE_CONF_FILE_ENV, "FENICE_CONF_FILE",[Define the name for the ENV variable for Fenice configuration file])

AC_DEFINE_UNQUOTED(FENICE_LOG_FILE_DEFAULT, $fenice_log_file, [Define default file for Fenice logger])

AC_DEFINE_UNQUOTED(FENICE_LOG_FILE_DEFAULT_STR, "$fenice_log_file", [Define default string for Fenice log file])

AC_DEFINE_UNQUOTED(FENICE_CONF_DIR_DEFAULT, $fenice_conf_dir, [Define default directory for Fenice configuration])
dnl AC_DEFINE(FENICE_CONF_DIR_ENV, "FENICE_CONF_DIR",[Define the name for the ENV variable for Fenice configuration dir])

AC_DEFINE_UNQUOTED(FENICE_CONF_PATH_DEFAULT_STR, "$fenice_conf_dir/$fenice_conf_file", [Define absolute path string for Fenice configuration file])
dnl AC_DEFINE(FENICE_CONF_PATH_STR_ENV, "FENICE_CONF_PATH",[Define the name for the ENV variable for Fenice configuration file path string])


AC_CONFIG_FILES([Makefile])
AC_CONFIG_FILES([eventloop/Makefile])
AC_CONFIG_FILES([rtp/Makefile])
AC_CONFIG_FILES([rtcp/Makefile])
AC_CONFIG_FILES([intnet/Makefile])
AC_CONFIG_FILES([rtsp/Makefile])
AC_CONFIG_FILES([mediainfo/Makefile])
AC_CONFIG_FILES([sdp/Makefile])
AC_CONFIG_FILES([src/Makefile])
AC_CONFIG_FILES([md5/Makefile])
AC_CONFIG_FILES([socket/Makefile])
AC_CONFIG_FILES([schedule/Makefile])
AC_CONFIG_FILES([xmlparser/Makefile])
AC_CONFIG_FILES([multicast/Makefile])
AC_CONFIG_FILES([command_environment/Makefile])
AC_CONFIG_FILES([docs/Makefile])
AC_CONFIG_FILES([docs/howto/Makefile])
AC_CONFIG_FILES([etc/Makefile])
AC_CONFIG_FILES([avroot/Makefile])
AC_CONFIG_FILES([bufferpool/Makefile])
AC_OUTPUT

AC_MSG_NOTICE([config files successfully generated!
 
Fenice Install prefix .......... : $prefix
Fenice Binary path ............. : $exec_prefix/bin/fenice
Fenice Configuration path .......: $fenice_conf_dir
Fenice Log File .................: $fenice_log_file
Fenice Multimedia Files root ....: $fenice_avroot_dir
Fenice RTSP listening port ......: $fenice_rtsp_port
$debug_msg$efence_msg$verbose_msg$dump_msg$dumpbuff_msg

 'make' will now compile Fenice and 'su -c make install' will install it.
 Note: On non-Linux systems you might need to use 'gmake' instead of 'make'.
])
dnl

