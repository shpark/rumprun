include ../../global.mk
include ${BUILDRUMP_TOOLFLAGS}

default: all

ARCHDIR?= ${MACHINE}
HW_MACHINE_ARCH?= ${MACHINE_GNU_ARCH}

LDSCRIPT:=	$(abspath arch/${ARCHDIR}/kern.ldscript)
SRCS+=		init.c

include ../Makefile.inc
include arch/${ARCHDIR}/Makefile.inc

# Disable PIE, but need to check if compiler supports it
LDFLAGS-$(call cc-option,-no-pie) += -no-pie
LDFLAGS += $(LDFLAGS-y)

OBJS:=	$(patsubst %.c,${RROBJ}/platform/%.o,${SRCS}) \
	$(patsubst %.S,${RROBJ}/platform/%.o,${ASMS})

.PHONY:	clean cleandir all

all:  links ${TARGETS}

${RROBJ}/include/nrk/machine:
	@mkdir -p ${RROBJ}/include/nrk
	@ln -sf $(shell pwd)/include/arch/${HW_MACHINE_ARCH} $@
	#@ln -sf $(shell pwd)/include/arch/${HW_MACHINE_ARCH}/md.h $@
	#@ln -sf $(shell pwd)/include/arch/${HW_MACHINE_ARCH}/pcpu.h $@

${RROBJ}/include/bmk-pcpu:
	@ln -sf ${RROBJ}/include/nrk/machine $@

links: ${RROBJ}/include/nrk/machine ${RROBJ}/include/bmk-pcpu

# ${RROBJ}/platform/%.o: %.c
# 	${CC} ${CPPFLAGS} ${CFLAGS} -c $< -o $@

# ${RROBJ}/platform/%.o: %.S
# 	${CC} -D_LOCORE ${CPPFLAGS} ${CFLAGS} -c $< -o $@

# ${MAINOBJ}: ${OBJS} platformlibs
# 	${CC} -nostdlib ${CFLAGS} ${LDFLAGS} -Wl,-r ${OBJS} -o $@ \
# 	    -L${RROBJLIB}/libbmk_core -L${RROBJLIB}/libbmk_rumpuser \
# 	    -Wl,--whole-archive -lbmk_rumpuser -lbmk_core -Wl,--no-whole-archive
# 	${OBJCOPY} -w -G bmk_* -G rumpuser_* -G jsmn_* \
# 	    -G rumprun_platform_rumpuser_init -G _start $@

clean: commonclean
	rm -f ${OBJS_BMK} include/nrk/machine buildtest ${MAINOBJ}

cleandir: clean
