artifacts_dir = ../../../../../artifacts
this_dir = .
top_srcdir = ../..
include_dir = $(top_srcdir)/include
arch_dir = $(top_srcdir)/../LwIP/system/
lwip_dir = $(top_srcdir)/../LwIP/src/include/
LIBCOAP_API_VERSION = $(shell sed -ne 's/^LIBCOAP_API_VERSION=\([0-9]\+\).*$$/\1/p' $(top_srcdir)/configure.ac)

coap_include_dir = $(include_dir)/coap$(LIBCOAP_API_VERSION)

CC=arm-none-eabi-gcc

all: libcoap	

# coap library
CFLAGS += -DWITH_LWIP -iquote.
CFLAGS += -std=gnu99

# set path to coap_include_dir for lwippools.h
CFLAGS += -I$(top_srcdir)/include -I$(coap_include_dir) -I$(this_dir)/lwip/include -I$(this_dir) -I$(lwip_dir) -I$(arch_dir)

vpath %.c $(top_srcdir)/src

COAPOBJS = net.o coap_cache.o coap_debug.o option.o resource.o pdu.o encode.o subscribe.o coap_io_lwip.o block.o uri.o str.o coap_session.o coap_notls.o coap_hashkey.o address.o coap_tcp.o async.o

CFLAGS += -g3 -Wall -Wextra -pedantic -O0
# not sorted out yet
CFLAGS += -Wno-unused-parameter

CFLAGS += -Ilwip/src/include/ -Ilwip/src/include/ipv4/

OBJS = ${COAPOBJS}

$(coap_include_dir)/coap.h:
	@echo "Error: $@ not present. Run the autotools chain (\`./autogen.sh && ./configure\`) in the project root directory to build the required coap.h file."
	@exit 1

${OBJS}: $(coap_include_dir)/coap.h

libcoap: ${OBJS}

clean:
	rm -f ${OBJS} libcoap.a

install:
	ar -rc libcoap.a ${OBJS} && mv libcoap.a $(artifacts_dir)/
.PHONY: clean
