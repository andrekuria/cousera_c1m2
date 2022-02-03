#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

TARGET = c1m2

# Platform Overrides

ifeq	($(PLATFORM),MSP432)

	# Architectures Specific Flags
	LINKER_FILE = ../msp432p401r.lds
	CPU = cortex-m4 
	ARCH = thumb
	SPECS = nosys.specs

	# Compiler Flags and Defines
	CC = arm-none-eabi-gcc 
	LD = arm-none-eabi-ld
	LDFLAGS = -Xlinker -Map=$(TARGET).map -T $(LINKER_FILE)
	CFLAGS = -mcpu=$(CPU) -m$(ARCH) -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16 --specs=$(SPECS) -Wall -Werror -g -O0 -std=c99
	CPPFLAGS = -MD
	PLATFORM_FLAG = -DMSP432
	
else    
	#FLAGS FOR WHEN THE PLATFORM IS HOST 
	PLATFORM = HOST
	PLATFORM_FLAG = -DHOST
	CC = gcc
	LD = ld
	LDFLAGS = -Xlinker -Map=$(TARGET).map
	CFLAGS = -Wall -Werror -g -O0 -std=c99 
	CPPFLAGS = -MD 
endif 



#PRODUCING THE PREPROCESSED FILES 

%i : $(SOURCES) 

	$(CC) -E $(CFLAGS) $(CPPFLAGS) -o $@ $(INCLUDES)  $(PLATFORM_FLAG) $< 
	
	
		
%.asm : $(SOURCES)

	$(CC) -S $(CFLAGS) $(CPPFLAGS) -o $@ $(INCLUDES) $(PLATFORM_FLAG) $<
	objdump -d $(TARGET).out 
	

%o : $(SOURCES) 

	$(CC) -c $(CFLAGS) $(CPPFLAGS) -o $@ $(INCLUDES)  $(PLATFORM_FLAG) $< 	
	

.PHONY : compile-all

compile-all:
	$(CC) -c $(CFLAGS) $(CPPFLAGS) $(INCLUDES)  $(PLATFORM_FLAG) $(SOURCES)


.PHONY : build  

build: 
	$(CC)  $(CFLAGS) $(CPPFLAGS) $(LDFLAGS)  -o $(TARGET).out  $(INCLUDES)  $(PLATFORM_FLAG) $(SOURCES)
	size $(TARGET).out
	
.PHONY : clean	
	
clean :

	rm -f *.i *.d *.map *.s *.out *.o *.asm

