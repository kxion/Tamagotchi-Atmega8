

MMCU_GCC     = atmega328p
MMCU_AVRDUDE = m328p
DEVICE    = /dev/ttyUSB0
INTERFACE = stk500v1
SPEED     = 19200
FREQ_CPU  = 16000000

HFUSE = 0xDD
LFUSE = 0xFF
EFUSE = 0x00

CC   = avr-gcc
DIR  = ../Code/
CFLAGS := -mmcu=$(MMCU_GCC) -Os -std=c99 -DF_CPU=$(FREQ_CPU)UL
CFLAGS_AVRDUDE = -c $(INTERFACE) -p $(MMCU_AVRDUDE) -P $(DEVICE) -b $(SPEED)

prog: main.o lcd.o rtc.o time.o
	$(CC) $(CFLAGS) -o prog main.o lcd.o rtc.o time.o

main.o: $(DIR)main.c $(DIR)defines.h $(DIR)rtc/rtc.h $(DIR)lcd/nokia5110.h
	$(CC) $(CFLAGS) -c $(DIR)main.c

rtc.o: $(DIR)rtc/rtc.c $(DIR)rtc/rtc.h
	$(CC) $(CFLAGS) -c $(DIR)rtc/rtc.c
time.o: $(DIR)time.c $(DIR)time.h $(DIR)rtc/rtc.h
	$(CC) $(CFLAGS) -c $(DIR)time.c
lcd.o: $(DIR)lcd/lcd.c $(DIR)lcd/nokia5110.h $(DIR)lcd/nokia5110_chars.h
	$(CC) $(CFLAGS) -c $(DIR)lcd/lcd.c

hex: prog
	avr-objcopy -O ihex prog prog.hex
upload: hex
	avrdude $(CFLAGS_AVRDUDE) -U flash:w:prog.hex

lfuse:
	avrdude $(CFLAGS_AVRDUDE) -U lfuse:w:$(LFUSE):m
hfuse:
	avrdude $(CFLAGS_AVRDUDE) -U hfuse:w:$(HFUSE):m
efuse:
	avrdude $(CFLAGS_AVRDUDE) -U efuse:w:$(EFUSE):m

fuses: lfuse hfuse efuse

clean:
	rm *.o