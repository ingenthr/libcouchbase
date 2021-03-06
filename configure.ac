#
#     Copyright 2010 Couchbase, Inc.
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.
#

#
# Configure script to check for all dependencies needed by libcouchbase.
# I prefer using the macros from the "pandora build system" where I can
# since they adds increased flexibility for searching for libraries and
# support multi-isa target systems.
#
# @author Trond Norbye
#
AC_PREREQ(2.60)
#m4_include([m4/version.m4])
AC_INIT(libcouchbase, VERSION_NUMBER, trond.norbye@gmail.com)
AC_CONFIG_SRCDIR([src/internal.h])
AC_CONFIG_AUX_DIR(config)

PANDORA_CANONICAL_TARGET(warnings-always-on)
PANDORA_REQUIRE_LIBVBUCKET
PANDORA_REQUIRE_LIBEVENT
PANDORA_REQUIRE_SASL

dnl The malloc tests seems to be broken for cross compilation. I'm pretty
dnl sure that all interesting platforms got a working malloc these days.
ac_cv_func_malloc_0_nonnull=yes
ac_cv_func_realloc_0_nonnull=yes

AC_SEARCH_LIBS(socket, socket)
AC_SEARCH_LIBS(gethostbyname, nsl)

AC_CHECK_HEADERS_ONCE([sys/socket.h
                       netinet/in.h
                       inttypes.h
                       netdb.h
                       unistd.h
                       ws2tcpip.h
                       winsock2.h])

AC_CACHE_CHECK([for htonll], [ac_cv_have_htonll],
    [AC_TRY_LINK([
#include <sys/types.h>
#include <netinet/in.h>
#include <inttypes.h>
                 ], [
return htonll(0);
                 ],
                 [ ac_cv_have_htonll=yes ],
                 [ ac_cv_have_htonll=no ])
])
AS_IF([test "x$ac_cv_have_htonll" = "xyes"],[
       AC_DEFINE([HAVE_HTONLL], [1], [Have ntohll])])


AH_TOP([
#ifndef CONFIG_H
#define CONFIG_H
/* -*- Mode: C; tab-width: 4; c-basic-offset: 4; indent-tabs-mode: nil -*- */
/*
 *     Copyright 2010 Couchbase, Inc.
 *
 *   Licensed under the Apache License, Version 2.0 (the "License");
 *   you may not use this file except in compliance with the License.
 *   You may obtain a copy of the License at
 *
 *       http://www.apache.org/licenses/LICENSE-2.0
 *
 *   Unless required by applicable law or agreed to in writing, software
 *   distributed under the License is distributed on an "AS IS" BASIS,
 *   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 *   See the License for the specific language governing permissions and
 *   limitations under the License.
 */

/*
 * This file is generated by running configure. Any changes you make to this
 * file will be overwritten the next time you run configure. If you want to
 * make permanent changes to the file you should edit configure.ac instead.
 * All platform-specific includes should be placed inside config_static.h
 * to keep the config.h as small as possible. That allows us for easily
 * use another build systems with a poor support for automake (like Windows)
 *
 * @author Trond Norbye
 */
])

AM_CPPFLAGS="$AM_CPPFLAGS -I\${top_srcdir}/include"

AH_BOTTOM([
#include "config_static.h"
#endif
])

dnl ----------------------------------------------------------------------------

AC_CONFIG_FILES(Makefile)
AC_OUTPUT
echo "---"
echo "Configuration summary for $PACKAGE_NAME version $VERSION"
echo ""
echo "   * Installation prefix:       $prefix"
echo "   * System type:               $host_vendor-$host_os"
echo "   * Host CPU:                  $host_cpu"
echo "   * C Compiler:                $CC_VERSION"
echo "   * Assertions enabled:        $ac_cv_assert"
echo "   * Debug enabled:             $with_debug"
echo "   * Warnings as failure:       $ac_cv_warnings_as_errors"
echo ""
echo "---"
