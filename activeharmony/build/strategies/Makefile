TO_BASE=../..
PREFIX=$(TO_BASE)
INST_DIR=$(PREFIX)/libexec

CC=gcc
CFLAGS=-std=c99 -pedantic -Wall -Werror -g

override CFLAGS+=-fPIC
override CPPFLAGS+=-I.. -D_ISOC99_SOURCE -D_XOPEN_SOURCE=500

TCL_INCDIR?=/usr/include
TCL_LIBDIR?=/usr/lib64

LIBEXEC_SRCS=libvertex.c \
             brute.c \
             random.c \
             nm.c \
             pro.c
SRCS=$(LIBEXEC_SRCS)

LIBEXEC_TARGETS=brute.so \
                random.so \
                nm.so \
                pro.so
TARGETS=$(LIBEXEC_TARGETS)

.PHONY: all install clean distclean

all: $(TARGETS)

install: $(TARGETS)
	@if [ ! -d $(INST_DIR) ]; then          \
		echo mkdir $(INST_DIR);         \
		mkdir $(INST_DIR);              \
	fi;                                     \
	if [ ! -d $(INST_DIR)/tcl ]; then       \
		echo mkdir $(INST_DIR)/tcl;     \
		mkdir $(INST_DIR)/tcl;          \
	fi;                                     \
	if [ ! -d $(INST_DIR)/tcl/pro ]; then   \
		echo mkdir $(INST_DIR)/tcl/pro; \
		mkdir $(INST_DIR)/tcl/pro;      \
	fi;                                     \
	for i in $(LIBEXEC_TARGETS); do         \
		echo cp $$i $(INST_DIR)/$$i;    \
		cp $$i $(INST_DIR)/$$i;         \
	done

brute.so: override LDLIBS+=-lm
brute.so: libvertex.o

random.so: override LDLIBS+=-lm
random.so: libvertex.o

nm.so: override LDLIBS+=-lm
nm.so: libvertex.o

pro.so: override LDLIBS+=-lm
pro.so: libvertex.o

%.so: %.o
	$(CC) -shared $(LDFLAGS) $(CFLAGS) $^ $(LOADLIBES) $(LDLIBS) -o $@

clean:
	rm -f core a.out *.o $(TARGETS)

distclean: clean
	rm -f *~ *.d

#
# Auto dependency creation
#
%.d: %.c
	@rm -f $@; \
		$(CC) -MM $(CPPFLAGS) $< > $@.$$$$ 2>/dev/null; \
		sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
		rm -f $@.$$$$

-include $(SRCS:.c=.d)
