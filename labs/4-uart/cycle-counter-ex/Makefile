# Makefile for a single file.  Makes it really hard to break anything.
MAKEFLAGS +=  --no-print-directory

# we've had some make issues going cross windows/mac/linux, so we just do brute stupid
# to make things work: no fancy rules, just always recompile.
NAME = cycle-cnt-ex

# check that your path is defined.
ifndef CS140E_PATH
$(error CS140E_PATH is not set: this should contain the absolute path to where this directory is.  Define it in your shell's initialiation.  For example, .tcshrc for tcsh or .bashrc for bash)
endif

include $(CS140E_PATH)/libpi/includes.mk

# if you want to compile everything in a directory, do something like this.
#   SOURCES := $(wildcard ./*.c)
#   OBJS := $(patsubst %.c, %.o, $(SOURCES))
OBJS := $(NAME).o

# CFLAGS += -falign-loops=32
# if you want to add all the .h's as dependencies:
#   DEPS := $(wildcard ./*.h) $(LPI)
DEPS := $(LPI) ./Makefile  cycle-util.h 


all: libpi $(OBJS) $(NAME).bin  # run

libpi: 
	@make -s -C $(LPP)


$(OBJS): $(DEPS)

# <LPI> will get rebuilt if anything changes, so we don't need any other
# dependencies.
$(NAME).bin: $(MEMMAP) $(OBJS) $(LPI) ./Makefile
	$(LD) $(START) $(OBJS) -T $(LPP)/memmap -o $(NAME).elf  $(LPI)
	@$(OD) -D $(NAME).elf > $(NAME).list
	$(OCP) $(NAME).elf -O binary $(NAME).bin

run:
	my-install /dev/ttyUSB1 ./$(NAME).bin


tags: 
	ctags *.[chSs] */*.[chSs]

clean:
	rm -rf $(BUILD_DIR) $(NAME) *~ tags *.o *.bin *.elf *.list *.img Makefile.bak 
#	@make -f ./Makefile.unix NAME=$(NAME).fake clean

cleanall:
	make clean
	make -f ./Makefile.unix NAME=$(NAME).fake clean
	make -C ../../../libpi-fake clean
.PHONY: libpi all run tags clean  fake
