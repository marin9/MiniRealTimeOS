CC			= arm-none-eabi
MCPU		= -mcpu=arm1176jzf-s
CFLAGS		= -nostdlib -ffreestanding -Wall -Wextra -O2
LDFLAGS		= -nostdlib -lgcc
LINKER		= linker.ld
TARGET_ELF	= kernel.elf
TARGET_BIN	= kernel.img

#####################
#  Configuration
#####################
ASM	:= start.s
SRC	:= main.c
#SRC += test.c
# Peripheral ########
SRC	+= gpio.c
SRC	+= uart.c
SRC += i2c.c
SRC += spi.c
SRC	+= timer.c
SRC	+= pic.c
SRC += pwm.c
# OS & Library ######
SRC	+= os.c
SRC += string.c
SRC += ctype.c
# Devices ###########
SRC += kb.c
SRC += flash.c
SRC += ssd1306.c
#####################

INC		:= $(wildcard *.h)
OBJA	:= $(ASM:%.s=%.o)
OBJC	:= $(SRC:%.c=%.o)


$(TARGET_BIN): $(TARGET_ELF)
	@echo " Create\t\t" $(TARGET_BIN)
	@$(CC)-objcopy $(TARGET_ELF) -O binary $(TARGET_BIN)

$(TARGET_ELF): $(LINKER) $(OBJA) $(OBJC)
	@echo " Linking\t" $(TARGET_ELF)
	@$(CC)-gcc -T $(LINKER) $(OBJA) $(OBJC) -o $@ $(LDFLAGS)

$(OBJA): %.o : %.s $(INC)
	@echo " Compile\t" $<
	@$(CC)-gcc $(MCPU) $(CFLAGS) -c $< -o $@

$(OBJC): %.o : %.c $(INC)
	@echo " Compile\t" $<
	@$(CC)-gcc $(MCPU) $(CFLAGS) -c $< -o $@

clean:
	@echo " Clean"
	@-rm -rf *.o $(TARGET_ELF) $(TARGET_BIN)
