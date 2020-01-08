.PHONY: all
all: default

include ../../global.mk
include ${BUILDRUMP_TOOLFLAGS}

OBJ_DIR ?= $(CURDIR)/obj
include ../Makefile.inc

.PHONY: default
default: prepare links ${MAINOBJ} ${TARGETS}


# Disable PIE, but need to check if compiler supports it
LDFLAGS-$(call cc-option,-no-pie) += -no-pie
LDFLAGS += $(LDFLAGS-y)

CFLAGS += -fno-builtin
rump-src-y += init.c

RUMP_OBJS= $(patsubst %.c,$(OBJ_DIR)/%.o,$(rump-src-y))

$(OBJ_DIR)/%.o: %.c $(HDRS) $(EXTRA_DEPS)
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $@

${RROBJ}/include/hw/machine:
	@mkdir -p ${RROBJ}/include/hw
	@ln -sf $(shell pwd)/include/arch/${HW_MACHINE_ARCH} $@

${RROBJ}/include/bmk-pcpu:
	@ln -sf ${RROBJ}/include/hw/machine $@

links: ${RROBJ}/include/hw/machine ${RROBJ}/include/bmk-pcpu


.PHONY: prepare
prepare:
	mkdir -p $(OBJ_DIR)/lib

$(MAINOBJ): $(RUMP_OBJS) platformlibs
	$(CC) -Wl,-r $(CFLAGS) $(LDFLAGS) $(RUMP_OBJS) -nostdlib -o $@

.PHONY: clean arch_clean

clean:	commonclean
	rm -f $(OBJ_DIR)/*.o $(MAINOBJ)

cleandir: clean
