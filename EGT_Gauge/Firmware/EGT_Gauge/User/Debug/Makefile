# Preprocessor Compiler and Linker Flags 
# --specs=nano.specs uses the newlib-nano library 
CFLAGS =  -W -Wextra -Wshadow -Wdouble-promotion \
            -Wformat-truncation -fno-common --specs=nano.specs \
            -g3 -Os -ffunction-sections -fdata-sections \
            -mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 $(EXTRA_CFLAGS)

# Define the target MCU            
CFLAGS += -DSTM32F411xE

# STM32 Include directories
INCLUDES = -I../../Core/Inc
INCLUDES += -I/usr/local/bin/arm-gnu-toolchain/arm-none-eabi/include
INCLUDES += -I../../Drivers/CMSIS/Device/ST/STM32F4xx/Include
INCLUDES += -I../../Drivers/CMSIS/Include
INCLUDES += -I../../Drivers/STM32F4xx_HAL_Driver/Inc
INCLUDES += -I../../Drivers/STM32F4xx_HAL_Driver/Inc/Legacy
# User Include directories
INCLUDES += -I../LCD/Config
INCLUDES += -I../LCD/example
INCLUDES += -I../LCD/Fonts
INCLUDES += -I../LCD/GUI_DEV
INCLUDES += -I../LCD/image
INCLUDES += -I../LCD/LCD
CFLAGS += $(INCLUDES)

# Source Files. Wildcard function finds all .c files in the src directory
SOURCES = $(wildcard ../../Core/Src/*.c)
SOURCES += $(wildcard ../../Drivers/STM32F4xx_HAL_Driver/Src/*.c)
SOURCES += $(wildcard ../LCD/Config/*.c)
SOURCES += $(wildcard ../LCD/example/*.c)
SOURCES += $(wildcard ../LCD/Fonts/*.c)
SOURCES += $(wildcard ../LCD/GUI_DEV/*.c)
SOURCES += $(wildcard ../LCD/image/*.c)
SOURCES += $(wildcard ../LCD/LCD/*.c)
SOURCES += $(wildcard *.s)

# Linker flags
LDFLAGS_FLASH = -TSTM32F411RETX_FLASH.ld -nostartfiles -nostdlib -lc -lgcc -Wl,--gc-sections -Wl,-Map=$@.map

# Remove Command
ifeq ($(OS),Windows_NT)
  RM = cmd /C del /Q /F
else
  RM = rm -f
endif

# Default Build Target
build: firmware.bin

# Build ELF file
firmware.elf: $(SOURCES)
	arm-none-eabi-gcc $(SOURCES) $(CFLAGS) $(LDFLAGS_FLASH) $(LDFLAGS_RAM) -o $@

# Convert ELF to Binary
firmware.bin: firmware.elf
	arm-none-eabi-objcopy -O binary $< $@

# Flash Firmware
flash: firmware.bin
	st-flash --reset write $< 0x8000000

# Clean Build Files
clean:
	-$(RM) firmware.*




# $ du -h filename (shows the file size in bytes)

# STM32F411RETX_FLASH.ld:95: non constant or forward reference address expression for section .ARM
# The compiler error above was fixed with removing the READONLY define starting on line 88-123
# Since the linker script was created using the stmCubeIDE it was using a older version of
# the compiler.

# Compiler Error: in function `__libc_init_array':
# init.c:(.text.__libc_init_array+0x10): undefined reference to `_init'
# Fixed by manually placing a Weak definition of _init function on line 104 in startup_stm32f411retx.s
