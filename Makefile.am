ACLOCAL_AMFLAGS = -I m4
AM_LDFLAGS = @COVERAGE_LDFLAGS@
AM_CPPFLAGS = -Iprime_server
AM_CXXFLAGS = -Iprime_server
LIBTOOL_DEPS = @LIBTOOL_DEPS@
libtool: $(LIBTOOL_DEPS)
	$(SHELL) ./config.status libtool

# things for versioning
pkgconfig_DATA = libprime_server.pc
EXTRA_DIST = version.sh

# conditional test coverage
if ENABLE_COVERAGE
.PHONY: clean-coverage
clean-coverage:
	-find -name '*.gcda' -exec rm -rf {} \;
	-$(LCOV) --directory $(top_builddir) -z
	-rm -rf coverage.info coverage/

.PHONY: coverage-report
coverage-report: clean-coverage
	-$(MAKE) $(AM_MAKEFLAGS) -k check
	$(MAKE) $(AM_MAKEFLAGS) coverage/index.html

coverage.info:
	$(LCOV) --directory $(top_builddir) --base-directory $(top_builddir) --no-external --capture --output-file $@ --no-checksum --compat-libtool

coverage/index.html: coverage.info
	$(GENHTML) --prefix $(top_builddir) --output-directory $(@D) --title "Test Coverage" --legend --show-details $<


.PHONY: clean-gcno
clean-gcno:
	-find -name '*.gcno' -exec rm -rf {} \;

clean-local: clean-coverage clean-gcno
endif

# lib prime_server compilation etc
lib_LTLIBRARIES = libprime_server.la
nobase_include_HEADERS = \
	prime_server/prime_server.hpp \
	prime_server/zmq_helpers.hpp \
	prime_server/netstring_protocol.hpp \
	prime_server/http_protocol.hpp \
	prime_server/logging.hpp 
libprime_server_la_SOURCES = \
	src/prime_server.cpp \
	src/zmq_helpers.cpp \
	src/netstring_protocol.cpp \
	src/http_protocol.cpp
libprime_server_la_CPPFLAGS = $(DEPS_CFLAGS)
libprime_server_la_LIBADD = $(DEPS_LIBS)

#distributed executables
bin_PROGRAMS = \
	prime_serverd \
	prime_httpd \
	prime_proxyd \
	prime_workerd \
	prime_echod
prime_serverd_SOURCES = \
        src/prime_serverd.cpp
prime_serverd_CPPFLAGS = $(DEPS_CFLAGS)
prime_serverd_LDADD = $(DEPS_LIBS) libprime_server.la -lpthread
prime_httpd_SOURCES = \
        src/prime_httpd.cpp
prime_httpd_CPPFLAGS = $(DEPS_CFLAGS)
prime_httpd_LDADD = $(DEPS_LIBS) libprime_server.la -lpthread
prime_proxyd_SOURCES = \
        src/prime_proxyd.cpp
prime_proxyd_CPPFLAGS = $(DEPS_CFLAGS)
prime_proxyd_LDADD = $(DEPS_LIBS) libprime_server.la -lpthread
prime_workerd_SOURCES = \
        src/prime_workerd.cpp
prime_workerd_CPPFLAGS = $(DEPS_CFLAGS)
prime_workerd_LDADD = $(DEPS_LIBS) libprime_server.la -lpthread
prime_echod_SOURCES = \
        src/prime_echod.cpp
prime_echod_CPPFLAGS = $(DEPS_CFLAGS)
prime_echod_LDADD = $(DEPS_LIBS) libprime_server.la -lpthread

# tests
check_PROGRAMS = test/zmq test/netstring test/http
test_zmq_SOURCES = test/zmq.cpp
test_zmq_CPPFLAGS = $(DEPS_CFLAGS)
test_zmq_LDADD = $(DEPS_LIBS) libprime_server.la -lpthread
test_netstring_SOURCES = test/netstring.cpp
test_netstring_CPPFLAGS = $(DEPS_CFLAGS)
test_netstring_LDADD = $(DEPS_LIBS) libprime_server.la -lpthread
test_http_SOURCES = test/http.cpp
test_http_CPPFLAGS = $(DEPS_CFLAGS)
test_http_LDADD = $(DEPS_LIBS) libprime_server.la -lpthread


TESTS = $(check_PROGRAMS)
TEST_EXTENSIONS = .sh
SH_LOG_COMPILER = sh

test: check
