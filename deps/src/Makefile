# Makefile lifted from Clang.jl
# Copyright (c) 2012-: Isaiah Norton and [contributors](https://github.com/ihnorton/Clang.jl/graphs/contributors)

ifeq (exists, $(shell [ -e Make.user ] && echo exists ))
include Make.user
endif

TARGETDIR=../usr/lib
OBJS = pa_ringbuffer.o

# Figure out OS and architecture
OS=$(shell uname)
ifneq (,$(findstring MINGW,$(OS)))
  OS=WINNT
endif

# file extensions and platform-specific libs
ifeq ($(OS), WINNT)
  LIBS +=
  LDFLAGS += -shared
  INC +=
  SHACMD = sha256sum
  SHLIB_EXT = dll
else ifeq ($(OS), Darwin)
  LIBS +=
  INC +=
  LDFLAGS += -dynamiclib
  SHLIB_EXT = dylib
  SHACMD = shasum -a256
else
  LIBS +=
  INC +=
  LDFLAGS += -shared
  SHLIB_EXT = so
  SHACMD = sha256sum
endif

CFLAGS =-Wall -Wno-strict-aliasing -fno-omit-frame-pointer -fPIC
LDFLAGS +=
# LINUX_LIBS =-lrt
# LINUX_LDFLAGS =-rdynamic
# add the Homebrew.jl tree to the include dirs in case we used it for
# portaudio and libsndfile
# DARWIN_LDFLAGS =-L../../../Homebrew/deps/usr/lib
# DARWIN_INC =-I../../../Homebrew/deps/usr/include

TARGET=$(TARGETDIR)/pa_ringbuffer.$(SHLIB_EXT)

.PHONY: clean default

default: $(TARGET)

%.o: %.c Makefile
	$(CC) $< -fPIC -c -o $@ $(INC) $(CFLAGS)

$(TARGETDIR):
	mkdir -p $@

$(TARGET): $(OBJS) $(TARGETDIR) Makefile
	$(CC) $(OBJS) $(LDFLAGS) -o $@ $(LIBS)

clean:
	rm -f $(OBJS)
	rm -f $(TARGET)