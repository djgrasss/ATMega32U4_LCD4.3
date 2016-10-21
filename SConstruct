import os

#Initialize the environment
env = DefaultEnvironment(ENV = os.environ, tools = ['gcc', 'gnulink'])
env.Replace(CC="avr-gcc")
env.Replace(CXX="avr-g++")
env.Replace(PROGSUFFIX=".elf")

mcu = 'atmega32u4'
f_cpu = 16000000
cstandard = 'gnu99'

env.Replace(CCFLAGS=["-mmcu={}".format(mcu),
                     "-Os",
                     "-gdwarf-2",
                     "-DF_CPU={}UL".format(f_cpu),
                     "-std={}".format(cstandard),
                     "-funsigned-char",
                     "-funsigned-bitfields",
                     "-fpack-struct",
                     "-fshort-enums",
                     "-Wall",
                     "-Wa,-adhlns=${TARGET.dir}/${SOURCE.filebase}.lst",
                     "-Wstrict-prototypes"])

env.Replace(LINKFLAGS=[env['CCFLAGS'], "-Wl,--gc-sections,--relax"])

SConscript('src/SConscript', variant_dir='build', duplicate=0)

hex=env.Command('main.hex','build/main.elf','avr-objcopy -O ihex -R .eeprom -R .fuse -R .lock $SOURCE $TARGET')
bin=env.Command('main.bin','build/main.elf','avr-objcopy -O binary -R .eeprom -R .fuse -R .lock $SOURCE $TARGET')

Default(hex, bin)

env.Command('programbin',bin,'avrdude -p atmega32u4 -P usb -c avrispv2 -U flash:w:$SOURCE')
env.Command('programhex',hex,'avrdude -p atmega32u4 -P usb -c avrispv2 -U flash:w:$SOURCE')
