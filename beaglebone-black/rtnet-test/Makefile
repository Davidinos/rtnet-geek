# Tested with linaro toolchain (gcc-linaro-7.4.1-2019.02-x86_64_arm-linux-gnueabihf)
CC = arm-linux-gnueabihf-gcc
XENO_INSTALL = /home/laur/lucru/raspberry-pi/robertcnelson/ti-linux-kernel-dev/ignore/xenomai-userspace
RTNET_INSTALL = /home/laur/lucru/raspberry-pi/robertcnelson/rtnet-hiddeate2m

#XENO_CONFIG := /home/laur/lucru/raspberry-pi/robertcnelson/ti-linux-kernel-dev/ignore/xenomai-userspace/usr/xenomai/bin/xeno-config
#/usr/xenomai/bin/xeno-config
#CFLAGS := $(shell $(XENO_CONFIG)   --posix --alchemy --cflags)
#LDFLAGS := $(shell $(XENO_CONFIG)  --posix --alchemy --ldflags)

CFLAGS = -Wall -I$(XENO_INSTALL)/usr/xenomai/include/cobalt -I$(XENO_INSTALL)/usr/xenomai/include/alchemy
CFLAGS += -I$(XENO_INSTALL)/usr/xenomai/include/trank -I$(XENO_INSTALL)/usr/xenomai/include 
CFLAGS += -mcpu=cortex-a8 -mfpu=neon-vfpv4 -mfloat-abi=hard
#CFLAGS += -march=armv7-a -mfpu=vfpv4 -mfloat-abi=hard
CFLAGS += -D_GNU_SOURCE -D_REENTRANT -D__COBALT__ -D__COBALT_WRAP_ -fasynchronous-unwind-tables
CFLAGS += -I$(RTNET_INSTALL)/stack/include

LFLAGS = -lalchemy -lcopperplate $(XENO_INSTALL)/usr/xenomai/lib/xenomai/bootstrap.o
LFLAGS += -Wl,--no-as-needed -Wl,@$(XENO_INSTALL)/usr/xenomai/lib/cobalt.wrappers -Wl,@$(XENO_INSTALL)/usr/xenomai/lib/modechk.wrappers 
LFLAGS += -Wl,--wrap=main -Wl,--wrap=malloc -Wl,--wrap=free -Wl,--dynamic-list=$(XENO_INSTALL)/usr/xenomai/lib/dynlist.ld 
LFLAGS += -L$(XENO_INSTALL)/usr/xenomai/lib -lcobalt -lpthread -lrt -lmodechk
LFLAGS += -mcpu=cortex-a8 -mfpu=neon-vfpv4 -mfloat-abi=hard
#LFLAGS +=  -march=armv7-a -mfpu=vfpv4 -mfloat-abi=hard -Llib

LFLAGS_ALCHEMY = -lalchemy -ltrank

all: rtt-sender.out rtt-responder.out frag-ip.out rttcp-server.out rttcp-client.out tftpc.out
rtt-sender.out: rtt-sender.o
	$(CC) -o rtt-sender.out rtt-sender.o $(LFLAGS) 
rtt-sender.o: rtt-sender.c
	$(CC) -c -o rtt-sender.o rtt-sender.c $(CFLAGS) $(LIBCFLAGS)
rtt-responder.out: rtt-responder.o
	$(CC) -o rtt-responder.out rtt-responder.o $(LFLAGS)
rtt-responder.o: rtt-responder.c
	$(CC) -c -o rtt-responder.o rtt-responder.c $(CFLAGS) $(LIBCFLAGS)
frag-ip.out: frag-ip.o
	$(CC) -o frag-ip.out frag-ip.o $(LFLAGS) $(LFLAGS_ALCHEMY)
frag-ip.o: frag-ip.c
	$(CC) -c -o frag-ip.o frag-ip.c $(CFLAGS) $(LIBCFLAGS)
rttcp-server.out: rttcp-server.o
	$(CC) -o rttcp-server.out rttcp-server.o $(LFLAGS) 
rttcp-server.o: rttcp-server.c
	$(CC) -c -o rttcp-server.o rttcp-server.c $(CFLAGS) $(LIBCFLAGS)
rttcp-client.out: rttcp-client.o
	$(CC) -o rttcp-client.out rttcp-client.o $(LFLAGS) 
rttcp-client.o: rttcp-client.c
	$(CC) -c -o rttcp-client.o rttcp-client.c $(CFLAGS) $(LIBCFLAGS)
tftpc.out: tftpc.o
	$(CC) -o tftpc.out tftpc.o $(LFLAGS) 
tftpc.o: tftpc.c
	$(CC) -c -o tftpc.o tftpc.c $(CFLAGS) $(LIBCFLAGS)

.PHONY: clean all

clean:
	rm -rf *.out *.so *.o rtt-sender rtt-responder frag-ip rttcp-server rttcp-client
