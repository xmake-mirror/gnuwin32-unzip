# Makefile for UnZip, fUnZip and UnZipSFX for native Win32-Intel ports of gcc.
# Currently supported implementations: Cygnus/Win32 and MinGW32.
#
# First version: Cosmin Truta, Dec 1997.
# Last revision: Cosmin Truta, 28-Feb-2004.
#
# To use, do "make -f win32/makefile.gcc".

resdir = win32/res
VPATH = . win32/res

### Optional section

# Optional nonstandard preprocessor flags (as -DUSE_ZLIB or -DUSE_SMITH_CODE)
# should be added to the environment via "set LOCAL_UNZIP=-DFOO" or added
# to the declaration of LOCFLAGS here:
LOCFLAGS = $(LOCAL_UNZIP)

MAJOR=5
MINOR=51
VERSADD = -Wl,--major-image-version=$(MAJOR) -Wl,--minor-image-version=$(MINOR)

### Compiler-specific section

# ------------ GNU C ------------
CC = gcc

#AS = as
AS = $(CC)

#LD = ld
LD = $(CC)

AR = ar

RC = windres

# Quiet
CC_QUIET_OPT =
AS_QUIET_OPT = $(CC_QUIET_OPT)
LD_QUIET_OPT = $(CC_QUIET_OPT)

# Warnings
CC_WARN_OPT = -Wall
AS_WARN_OPT = $(CC_WARN_OPT)
LD_WARN_OPT =

# Debug version
CC_DEBUG_OPT = -g
AS_DEBUG_OPT = $(CC_DEBUG_OPT)
LD_DEBUG_OPT = $(CC_DEBUG_OPT)

# Release version
CC_RELEASE_OPT =
AS_RELEASE_OPT =
LD_RELEASE_OPT = -Wl,-s	$(VERSADD)

# Prefered target CPU (instruction scheduling optimized for...)
#ifndef CC_CPU_OPT
CC_CPU_OPT = -mcpu=pentiumpro
#endif

# Smallest code  (-Os is new since EGC 1.1, use -O1 for 2.8.1 and earlier)
CC_SIZE_OPT = -Os $(CC_CPU_OPT)

# Fastest code
CC_SPEED_OPT = -O3 $(CC_CPU_OPT)

# Output object file name
CC_OUT_OPT = -o

# Other specific options
CFLAGSEXTRA = -D__GW32__ -D_LARGEFILE_SOURCE=1 -D_LARGEFILE64_SOURCE=1 -D_FILE_OFFSET_BITS=64 \
	-Wall -O3 -fms-extensions -mms-bitfields  -fno-exceptions -fomit-frame-pointer \
	-march=i386 -mcpu=i686 \
	-idirafter e:/progra~1/gnuwin32/include -idirafter h:/glibc/include  \
	-DUNIXBACKUP -DUSE_UNSHRINK -DACORN_FTYPE_NFS -DOS2_EAS -DUSE_CRYPT
CC_SPECIFIC_OPT = -c -DASM_CRC -DWIN32 $(CFLAGSEXTRA)
AS_SPECIFIC_OPT = -c
LD_SPECIFIC_OPT = -o $@

# Libraries for the debug & release version
# (GCC 2.95 and newer does not require the following library specifications)
LD_RELEASE_LIBS = -lgw32c -lole32 -luuid -luser32 -ladvapi32
LD_DEBUG_LIBS = $(LD_RELEASE_LIBS)


### System-specific section

# Suffixes
OBJ = .o
EXE = .exe

.SUFFIXES: .c .S $(OBJ) $(EXE) .rc

# Commands
RM = rm -f


### General section

CFLAGS  = $(CC_SPECIFIC_OPT) $(CC_QUIET_OPT) $(CC_WARN_OPT) $(LOCFLAGS) \
 $(CC_OUT_OPT) $@
ASFLAGS = $(AS_SPECIFIC_OPT) $(AS_QUIET_OPT) $(AS_WARN_OPT) $(LOCFLAGS)
LDFLAGS = $(LD_SPECIFIC_OPT) $(LD_QUIET_OPT) $(LD_WARN_OPT)

# To build with debug info, use 'make DEBUG=1'.
ifdef DEBUG
CVER     = $(CC_DEBUG_OPT)
ASVER    = $(AS_DEBUG_OPT)
LDVER    = $(LD_DEBUG_OPT)
GENFLAGS =
FFLAGS   = -DFUNZIP
SFXFLAGS = -DSFX
GENDLLFL = -DDLL -DWINDLL
GENGUILB = -DSFX -DDLL -DWINDLL -DUNZIPLIB
GENGUISX = -DSFX
GENLIBFL = -DDLL -DWINDLL -DUNZIPLIB
LDLIBS   = $(LD_DEBUG_LIBS)
else
CVER     = $(CC_RELEASE_OPT)
ASVER    = $(AS_RELEASE_OPT)
LDVER    = $(LD_RELEASE_OPT)
GENFLAGS = $(CC_SPEED_OPT)
FFLAGS   = $(CC_SPEED_OPT) -DFUNZIP
SFXFLAGS = $(CC_SIZE_OPT) -DSFX
GENDLLFL = $(CC_SPEED_OPT) -DDLL -DWINDLL
GENGUILB = $(CC_SIZE_OPT) -DSFX -DDLL -DWINDLL -DUNZIPLIB
GENGUISX = $(CC_SIZE_OPT) -DSFX
GENLIBFL = $(CC_SPEED_OPT) -DUNZIPLIB
LDLIBS   = $(LD_RELEASE_LIBS)
endif
GUILDFLAG=-mwindows

# Object files
OBJA  = crc_i386$(OBJ)
OBJU1 = unzip$(OBJ) crc32$(OBJ) crctab$(OBJ) crypt$(OBJ) envargs$(OBJ)
OBJU2 = explode$(OBJ) extract$(OBJ) fileio$(OBJ) globals$(OBJ) inflate$(OBJ)
OBJU3 = list$(OBJ) match$(OBJ) process$(OBJ) ttyio$(OBJ) unreduce$(OBJ)
OBJU4 = unshrink$(OBJ) zipinfo$(OBJ)
OBJUS = win32$(OBJ) nt$(OBJ)
OBJU  = $(OBJU1) $(OBJU2) $(OBJU3) $(OBJU4) $(OBJA) $(OBJUS) unzip-res$(OBJ)
OBJI  = $(OBJU:unzip-res.o=zipinfo-res.o)
OBJX1 = unzipsfx$(OBJ) crc32x$(OBJ) crctabx$(OBJ) cryptx$(OBJ) extractx$(OBJ)
OBJX2 = fileiox$(OBJ) globalsx$(OBJ) inflatex$(OBJ) matchx$(OBJ) processx$(OBJ)
OBJX3 = ttyiox$(OBJ)
OBJXS = win32x$(OBJ) ntx$(OBJ)
OBJX  = $(OBJX1) $(OBJX2) $(OBJX3) $(OBJA) $(OBJXS) unzipsfx-res$(OBJ)
OBJF1 = funzip$(OBJ) crc32$(OBJ) cryptf$(OBJ) globalsf$(OBJ) inflatef$(OBJ)
OBJF2 = ttyiof$(OBJ)
OBJFS = win32f$(OBJ)
OBJF  = $(OBJF1) $(OBJF2) $(OBJA) $(OBJFS) funzip-res$(OBJ)
OBJDLL = windll$(OBJ) unzip-dll-res$(OBJ) api$(OBJ) unzip-dllversion$(OBJ)
OBJD1 = crc32l$(OBJ) crctabl$(OBJ) cryptl$(OBJ)
OBJD2 = explodel$(OBJ) extractl$(OBJ) fileiol$(OBJ) globalsl$(OBJ)
OBJD3 = inflatel$(OBJ) listl$(OBJ) matchl$(OBJ) processl$(OBJ)
OBJD4 = unreducl$(OBJ) unshrnkl$(OBJ) zipinfol$(OBJ)
OBJDS = win32l$(OBJ) ntl$(OBJ)
OBJD  = $(OBJDLL) $(OBJD1) $(OBJD2) $(OBJD3) $(OBJD4) $(OBJA) $(OBJDS)
OBLX1 = apig$(OBJ) crc32g$(OBJ) crctabg$(OBJ) cryptg$(OBJ)
OBLX2 = extractg$(OBJ) fileiog$(OBJ) globalsg$(OBJ) inflateg$(OBJ)
OBLX3 = matchg$(OBJ) processg$(OBJ)
OBLXS = win32g$(OBJ) ntg$(OBJ) windllg$(OBJ)
OBLX  = $(OBLX1) $(OBLX2) $(OBLX3) $(OBJA) $(OBLXS)
OBGX  = sfxwiz$(OBJ) sfxwizrc$(OBJ) SFXWiz32-res$(OBJ)
OBJLIB = apib$(OBJ)
OBJB1 = crc32b$(OBJ) crctabb$(OBJ) cryptb$(OBJ)
OBJB2 = explodeb$(OBJ) extractb$(OBJ) fileiob$(OBJ) globalsb$(OBJ)
OBJB3 = inflateb$(OBJ) listb$(OBJ) matchb$(OBJ) processb$(OBJ)
OBJB4 = unreducb$(OBJ) unshrnkb$(OBJ) zipinfob$(OBJ)
OBJBS = win32b$(OBJ) ntb$(OBJ)
OBJB  = $(OBJLIB) $(OBJB1) $(OBJB2) $(OBJB3) $(OBJB4) $(OBJA) $(OBJBS)

UNZIP_H = unzip.h unzpriv.h globals.h win32/w32cfg.h
WINDLL_H = windll/windll.h windll/structs.h windll/decs.h
DLLDEF = windll/windllgcc.def
WINDLL_IMP_H = windll/decs.h windll/structs.h


# Default target is all the executables
all:    unzips dll dllsample guisfx lib
unzips: unzip$(EXE) funzip$(EXE) unzipsfx$(EXE) zipinfo$(EXE)
dll:    unzip32.dll
dllsample: uzexampl$(EXE)
guisfx: SFXWiz32$(EXE)
lib:    libunzip32.a

unzip$(EXE): $(OBJU)
	$(LD) $(LDFLAGS) $(LDVER) $(OBJU) $(LDLIBS)

unzipsfx$(EXE): $(OBJX)
	$(LD) $(LDFLAGS) $(LDVER) $(OBJX) $(LDLIBS)

funzip$(EXE): $(OBJF)
	$(LD) $(LDFLAGS) $(LDVER) $(OBJF) $(LDLIBS)

zipinfo$(EXE): $(OBJI)
	$(LD) $(LDFLAGS) $(LDVER) $(OBJI) $(LDLIBS)

unzip32.dll: $(DLLDEF) $(OBJD)
#	dllwrap --driver-name $(CC) --def $(DLLDEF) $(LDFLAGS) $(LDVER) $(OBJD) $(LDLIBS)
	o2dll.sh -o $@ -l libunzip32 -d $(DLLDEF) $(OBJD) -Rwin32/res $(LDVER) $(LDLIBS)
	
libunzsfx32.a: $(OBLX)
	$(AR) -rus $@ $(OBLX)

SFXWiz32$(EXE): $(OBGX) libunzsfx32.a
	$(LD) $(GUILDFLAG) $(LDFLAGS) $(LDVER) $(OBGX) -L. -lunzsfx32 $(LDLIBS)

uzexampl$(EXE): uzexampl$(OBJ) uzexampl-res$(OBJ)
	$(CC) $(LDFLAGS) $(LDVER) uzexampl$(OBJ) uzexampl-res$(OBJ) -lversion $(LDLIBS)

libunzip32.a: $(OBJB)
	$(AR) -rus $@ $(OBJB)


# How to compile sources
.c$(OBJ):
	$(CC) $(CFLAGS) $(CVER) $(GENFLAGS) $<
.S$(OBJ):
	$(AS) $(ASFLAGS) $(ASVER) $(GENFLAGS) $<

# Rule to make compiled resource (Windows)
.rc$(OBJ):
	windres --include-dir $(resdir) -i $< -o $@


# Dependencies
crc32$(OBJ):    crc32.c $(UNZIP_H) zip.h
crctab$(OBJ):   crctab.c $(UNZIP_H) zip.h
crypt$(OBJ):    crypt.c $(UNZIP_H) zip.h crypt.h ttyio.h
envargs$(OBJ):  envargs.c $(UNZIP_H)
explode$(OBJ):  explode.c $(UNZIP_H)
extract$(OBJ):  extract.c $(UNZIP_H) crypt.h
fileio$(OBJ):   fileio.c $(UNZIP_H) crypt.h ttyio.h ebcdic.h
funzip$(OBJ):   funzip.c $(UNZIP_H) crypt.h ttyio.h tables.h
globals$(OBJ):  globals.c $(UNZIP_H)
inflate$(OBJ):  inflate.c inflate.h $(UNZIP_H)
list$(OBJ):     list.c $(UNZIP_H)
match$(OBJ):    match.c $(UNZIP_H)
process$(OBJ):  process.c $(UNZIP_H)
ttyio$(OBJ):    ttyio.c $(UNZIP_H) zip.h crypt.h ttyio.h
unreduce$(OBJ): unreduce.c $(UNZIP_H)
unshrink$(OBJ): unshrink.c $(UNZIP_H)
unzip$(OBJ):    unzip.c $(UNZIP_H) crypt.h unzvers.h consts.h
zipinfo$(OBJ):  zipinfo.c $(UNZIP_H)
crc_i386$(OBJ): crc_i386.S

win32$(OBJ): win32/win32.c $(UNZIP_H) win32/nt.h
	$(CC) $(CFLAGS) $(CVER) $(GENFLAGS) -I. win32/win32.c

nt$(OBJ): win32/nt.c $(UNZIP_H) win32/nt.h
	$(CC) $(CFLAGS) $(CVER) $(GENFLAGS) -I. win32/nt.c

win32x$(OBJ): win32/win32.c $(UNZIP_H) win32/nt.h		# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) -I. win32/win32.c

ntx$(OBJ): win32/nt.c $(UNZIP_H) win32/nt.h			# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) -I. win32/nt.c

win32f$(OBJ): win32/win32.c $(UNZIP_H) win32/nt.h		# funzip only
	$(CC) $(CFLAGS) $(CVER) $(FFLAGS) -I. win32/win32.c

crc32x$(OBJ): crc32.c $(UNZIP_H) zip.h				# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) crc32.c

crctabx$(OBJ): crctab.c $(UNZIP_H) zip.h			# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) crctab.c

cryptx$(OBJ): crypt.c $(UNZIP_H) zip.h crypt.h ttyio.h		# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) crypt.c

cryptf$(OBJ): crypt.c $(UNZIP_H) zip.h crypt.h ttyio.h		# funzip only
	$(CC) $(CFLAGS) $(CVER) $(FFLAGS) crypt.c

extractx$(OBJ): extract.c $(UNZIP_H) crypt.h			# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) extract.c

fileiox$(OBJ):   fileio.c $(UNZIP_H) crypt.h ttyio.h ebcdic.h	# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) fileio.c

globalsx$(OBJ): globals.c $(UNZIP_H)				# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) globals.c

globalsf$(OBJ): globals.c $(UNZIP_H)				# funzip only
	$(CC) $(CFLAGS) $(CVER) $(FFLAGS) globals.c

inflatex$(OBJ): inflate.c inflate.h $(UNZIP_H) crypt.h		# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) inflate.c

inflatef$(OBJ): inflate.c inflate.h $(UNZIP_H) crypt.h		# funzip only
	$(CC) $(CFLAGS) $(CVER) $(FFLAGS) inflate.c

matchx$(OBJ): match.c $(UNZIP_H)				# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) match.c

processx$(OBJ): process.c $(UNZIP_H)				# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) process.c

ttyiox$(OBJ): ttyio.c $(UNZIP_H) zip.h crypt.h ttyio.h		# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) ttyio.c

ttyiof$(OBJ): ttyio.c $(UNZIP_H) zip.h crypt.h ttyio.h		# funzip only
	$(CC) $(CFLAGS) $(CVER) $(FFLAGS) ttyio.c

unzipsfx$(OBJ): unzip.c $(UNZIP_H) crypt.h unzvers.h consts.h	# unzipsfx only
	$(CC) $(CFLAGS) $(CVER) $(SFXFLAGS) unzip.c

uzexampl$(OBJ):	windll/uzexampl.c windll/uzexampl.h		# WINDLL sample
	$(CC) $(CFLAGS) $(CVER) $(GENFLAGS) -I. windll/uzexampl.c

# DLL compilation section
api$(OBJ): api.c $(UNZIP_H) $(WINDLL_H) unzvers.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) api.c

crc32l$(OBJ):	crc32.c $(UNZIP_H) zip.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) crc32.c

crctabl$(OBJ):	crctab.c $(UNZIP_H) zip.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) crctab.c

cryptl$(OBJ):	crypt.c $(UNZIP_H) zip.h crypt.h ttyio.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) crypt.c

explodel$(OBJ):	explode.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) explode.c

extractl$(OBJ):	extract.c $(UNZIP_H) $(WINDLL_H) crypt.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) extract.c

fileiol$(OBJ):	fileio.c $(UNZIP_H) $(WINDLL_H) crypt.h ttyio.h ebcdic.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) fileio.c

globalsl$(OBJ):	globals.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) globals.c

inflatel$(OBJ):	inflate.c inflate.h $(UNZIP_H) crypt.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) inflate.c

listl$(OBJ):	list.c $(UNZIP_H) $(WINDLL_H)
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) list.c

matchl$(OBJ):	match.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) match.c

processl$(OBJ):	process.c $(UNZIP_H) $(WINDLL_H)
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) process.c

unreducl$(OBJ):	unreduce.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) unreduce.c

unshrnkl$(OBJ):	unshrink.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) unshrink.c

zipinfol$(OBJ):	zipinfo.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) zipinfo.c

win32l$(OBJ): win32/win32.c $(UNZIP_H) win32/nt.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) -I. win32/win32.c

ntl$(OBJ): win32/nt.c $(UNZIP_H) win32/nt.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) -I. win32/nt.c

windll$(OBJ): windll/windll.c $(UNZIP_H) $(WINDLL_H) crypt.h unzvers.h consts.h
	$(CC) $(CFLAGS) $(CVER) $(GENDLLFL) -I. windll/windll.c

windll-res$(OBJ):	windll/windll.rc unzvers.h
	- $(RC) --include-dir . --include-dir $(resdir) -o $@ windll/windll.rc

# SFX Lib compilation section
apig$(OBJ):	api.c $(UNZIP_H) $(WINDLL_H) unzvers.h
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) api.c

crc32g$(OBJ):	crc32.c $(UNZIP_H) zip.h
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) crc32.c

crctabg$(OBJ):	crctab.c $(UNZIP_H) zip.h
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) crctab.c

cryptg$(OBJ):	crypt.c $(UNZIP_H) zip.h crypt.h ttyio.h
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) crypt.c

extractg$(OBJ):	extract.c $(UNZIP_H) $(WINDLL_H) crypt.h
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) extract.c

fileiog$(OBJ):	fileio.c $(UNZIP_H) $(WINDLL_H) crypt.h ttyio.h ebcdic.h
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) fileio.c

globalsg$(OBJ):	globals.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) globals.c

inflateg$(OBJ):	inflate.c inflate.h $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) inflate.c

matchg$(OBJ):	match.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) match.c

processg$(OBJ):	process.c $(UNZIP_H) $(WINDLL_H)
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) process.c

win32g$(OBJ):	win32/win32.c $(UNZIP_H) win32/nt.h
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) -I. win32/win32.c

ntg$(OBJ):	win32/nt.c $(UNZIP_H) win32/nt.h
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) -I. win32/nt.c

windllg$(OBJ):  windll/windll.c $(UNZIP_H) $(WINDLL_H) crypt.h unzvers.h consts.h
	$(CC) $(CFLAGS) $(CVER) $(GENGUILB) windll/windll.c

sfxwiz$(OBJ):	windll/guisfx/sfxwiz.c windll/guisfx/dialog.h $(WINDLL_IMP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENGUISX) -I. windll/guisfx/sfxwiz.c

sfxwizrc$(OBJ):	windll/guisfx/sfxwiz.rc
	- $(RC)  --include-dir windll/guisfx --include-dir . --include-dir $(resdir) --define WIN32 -o$@ \
		windll/guisfx/sfxwiz.rc

# Static LIB compilation section
apib$(OBJ): api.c $(UNZIP_H) $(WINDLL_H) unzvers.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) api.c

crc32b$(OBJ):	crc32.c $(UNZIP_H) zip.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) crc32.c

crctabb$(OBJ):	crctab.c $(UNZIP_H) zip.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) crctab.c

cryptb$(OBJ):	crypt.c $(UNZIP_H) zip.h crypt.h ttyio.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) crypt.c

explodeb$(OBJ):	explode.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) explode.c

extractb$(OBJ):	extract.c $(UNZIP_H) crypt.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) extract.c

fileiob$(OBJ):	fileio.c $(UNZIP_H) crypt.h ttyio.h ebcdic.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) fileio.c

globalsb$(OBJ):	globals.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) globals.c

inflateb$(OBJ):	inflate.c inflate.h $(UNZIP_H) crypt.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) inflate.c

listb$(OBJ):	list.c $(UNZIP_H) $(WINDLL_H)
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) list.c

matchb$(OBJ):	match.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) match.c

processb$(OBJ):	process.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) process.c

unreducb$(OBJ):	unreduce.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) unreduce.c

unshrnkb$(OBJ):	unshrink.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) unshrink.c

zipinfob$(OBJ):	zipinfo.c $(UNZIP_H)
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) zipinfo.c

win32b$(OBJ): win32/win32.c $(UNZIP_H) win32/nt.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) -I. win32/win32.c

ntb$(OBJ): win32/nt.c $(UNZIP_H) win32/nt.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) -I. win32/nt.c

windllb$(OBJ): windll/windll.c $(UNZIP_H) $(WINDLL_H) crypt.h unzvers.h consts.h
	$(CC) $(CFLAGS) $(CVER) $(GENLIBFL) -I. windll/windll.c

clean:
	$(RM) *$(OBJ)
	$(RM) unzip$(EXE) funzip$(EXE) unzipsfx$(EXE)
	$(RM) unzip32.dll uzexampl$(EXE) SFXWiz32$(EXE)
	$(RM) libunzip32.a libunzsfx32.a
	$(RM) libunzip32.dll.a libunzsfx32.dll.a
	$(RM) libunzip32.lib libunzip32-bcc.lib

# installation
# (probably can change next two to `install' and `install -d' if you have it)
INSTALL = /bin/install -dpD
INSTALL_PROGRAM = $(INSTALL)
INSTALL_D = mkdir -p
# on some systems, manext=l and MANDIR=/usr/man/man$(manext) may be appropriate
manext = 1
prefix = c:/progra~1/UnZip
BINDIR = $(prefix)/bin#            where to install executables
MANDIR = $(prefix)/man/man$(manext)#    where to install man pages
INSTALLEDBIN = $(BINDIR)/funzip$E $(BINDIR)/unzip$E $(BINDIR)/unzipsfx$E \
	$(BINDIR)/zipgrep$E $(BINDIR)/zipinfo$E
INSTALLEDMAN = $(MANDIR)/funzip.$(manext) $(MANDIR)/unzip.$(manext) \
	$(MANDIR)/unzipsfx.$(manext) $(MANDIR)/zipgrep.$(manext) \
	$(MANDIR)/zipinfo.$(manext)

install:  $(MANS)
	-$(INSTALL_D) $(BINDIR)
	$(INSTALL_PROGRAM) $(UNZIPS) $(BINDIR)
	$(INSTALL) unix/zipgrep $(BINDIR)
	$(RM) $(BINDIR)/zipinfo$E
	$(LN) $(BINDIR)/unzip$E $(BINDIR)/zipinfo$E
	-$(INSTALL_D) $(MANDIR)
	$(INSTALL) man/funzip.1 $(MANDIR)/funzip.$(manext)
	$(INSTALL) man/unzip.1 $(MANDIR)/unzip.$(manext)
	$(INSTALL) man/unzipsfx.1 $(MANDIR)/unzipsfx.$(manext)
	$(INSTALL) man/zipgrep.1 $(MANDIR)/zipgrep.$(manext)
	$(INSTALL) man/zipinfo.1 $(MANDIR)/zipinfo.$(manext)
	$(CHMOD) $(BINPERMS) $(INSTALLEDBIN)
	$(CHMOD) $(MANPERMS) $(INSTALLEDMAN)

uninstall:
	$(RM) $(INSTALLEDBIN) $(INSTALLEDMAN)

