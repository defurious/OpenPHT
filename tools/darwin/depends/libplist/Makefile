include ../Makefile.include
include ../config.site.mk
DEPS= Makefile 0001-dontbuildswig.patch 0002-fixclang.patch 0003-fixc++1.patch

# lib name, version
LIBNAME=libplist
VERSION=1.6
SOURCE=$(LIBNAME)-$(VERSION)
FILENAME=v$(VERSION)
ARCHIVE=$(SOURCE).tar.gz

LIBDYLIB=$(SOURCE)/build/src/$(LIBNAME).dylib

all: $(LIBDYLIB) .installed

$(TARBALLS_LOCATION)/$(ARCHIVE):
	$(RETRIEVE_TOOL) $(RETRIEVE_TOOL_FLAGS) $(BASE_URL)/$(ARCHIVE)

$(SOURCE): $(TARBALLS_LOCATION)/$(ARCHIVE) $(DEPS)
	rm -rf $(SOURCE)
	$(ARCHIVE_TOOL) $(ARCHIVE_TOOL_FLAGS) $(TARBALLS_LOCATION)/$(ARCHIVE)
	echo $(SOURCE) > .gitignore
	cd $(SOURCE); patch -p1 < ../0001-dontbuildswig.patch
	cd $(SOURCE); patch -p1 < ../0002-fixclang.patch
	cd $(SOURCE); patch -p1 < ../0003-fixc++1.patch
	cd $(SOURCE); rm -rf build; mkdir -p build
	cd $(SOURCE)/build; CC="$(CC)" CXX="$(CXX)" cmake        \
	  -DCMAKE_C_FLAGS:STRING="$(CPPFLAGS) $(CFLAGS)"     \
	  -DCMAKE_CXX_FLAGS:STRING="$(CPPFLAGS) $(CXXFLAGS)" \
	  -DCMAKE_INCLUDE_PATH=$(PREFIX)/include             \
	  -DCMAKE_LIBRARY_PATH=$(PREFIX)/lib                 \
	  -DCMAKE_INSTALL_NAME_DIR=$(PREFIX)/lib             \
	  -DCMAKE_INSTALL_PREFIX=$(PREFIX)                   \
	  -DCMAKE_FIND_ROOT_PATH=$(platform_sdk_path) ..

$(LIBDYLIB): $(SOURCE)
	make -j 1 -C $(SOURCE)/build

.installed:
	make -C $(SOURCE)/build install
	touch $@

clean:
	rm -rf $(SOURCE) .installed

distclean::
	rm -rf $(SOURCE) .installed

