include ../../../config.mk
#CC?=gcc
CC?=gcc
#OSTYPE=$(shell uname -s)
include ../../../../config-user.mk
RANLIB?=ranlib
ifeq (${OSTYPE},)
all:
	echo LE FAIL
	exit 1
endif

KERNFILES=kern/file.c 
KERNFILES+=kern/term.c kern/device.c
KERNFILES+=kern/err.c
KERNFILES+=kern/env.c kern/disk.c
KERNFILES+=kern/fs.c kern/misc.c
KERNFILES+=kern/time.c
KERNFILES+=kern/list.c kern/partition.c
KERNFILES+=kern/mm.c
#KERNFILES+=kern/dl.c
KERNFILES+=fs/fshelp.c
KERNFILES+=fs/reiserfs.c
KERNFILES+=fs/ext2.c
KERNFILES+=fs/fat.c
KERNFILES+=fs/ntfs.c
KERNFILES+=fs/ntfscomp.c
KERNFILES+=fs/cpio.c
KERNFILES+=fs/tar.c
KERNFILES+=fs/xfs.c 
KERNFILES+=fs/ufs.c 
KERNFILES+=fs/ufs2.c 
# All nested functions are removed from the following .c with a lot of tobacco :-)
KERNFILES+=fs/hfs.c 
KERNFILES+=fs/hfsplus.c 
KERNFILES+=fs/udf.c 
KERNFILES+=fs/iso9660.c
KERNFILES+=fs/minix.c
#KERNFILES+=fs/minix2.c #XXX: Are minix2 and minix the same?
KERNFILES+=fs/jfs.c
KERNFILES+=fs/fb.c
KERNFILES+=fs/sfs.c
KERNFILES+=grubfs.c

KERNFILES+=partmap/msdos.c
#KERNFILES+=partmap/acorn.c
KERNFILES+=partmap/gpt.c
KERNFILES+=partmap/apple.c
KERNFILES+=partmap/amiga.c
KERNFILES+=partmap/sun.c
KERNFILES+=partmap/bsdlabel.c
KERNFILES+=partmap/sunpc.c

KERNOBJS=$(subst .c,.o,${KERNFILES})
CFLAGS+=-Iinclude -fPIC 
CFLAGS+=-I../../../include -DGRUB_TARGET_NO_MODULES
CFLAGS+=-g

# This fixes a silly GNU gcc build problem in OSX - BLAME! :D
ifeq ($(OSTYPE),darwin)
CFLAGS+=-DAPPLE_CC
endif

BIN=test${EXT_EXE}

all: ${BIN}

${BIN}: ${KERNOBJS} main.o
	${CC} -o ${BIN} main.o ${CFLAGS} ${KERNOBJS}

lib: all libgrubfs.a

libgrubfs.a: ${KERNOBJS}
	rm -f libgrubfs.a
	ar -q libgrubfs.a ${KERNOBJS}
	${RANLIB} libgrubfs.a

clean:
	rm -f ${KERNOBJS} ${BIN} main.o
	rm -f libgrubfs.a

fs: /tmp/fs
	@echo ======================
	@echo m ext2 /mnt
	@echo md /mnt
	@echo mo /mnt/fstab
	@echo mg /mnt/fstab
	@echo q
	@echo ======================
	r2 -n /tmp/fs

/tmp/fs:
	rm -f /tmp/fs
	dd count=1 bs=4M if=/dev/zero of=/tmp/fs
	mkfs.ext2 /tmp/fs
	sudo mount -o loop /tmp/fs /media
	sudo cp /etc/fstab /media
	sudo cp /etc/resolv.conf /media
	sudo umount /media

sync:
	echo "XXX: Implement syncing with burg project"

.PHONY: fs clean all lib
