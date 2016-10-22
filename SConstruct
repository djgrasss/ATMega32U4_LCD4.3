import os

#Initialize the environment
env = DefaultEnvironment(ENV = os.environ, tools = ['gcc', 'gnulink'])
env.Replace(CC='avr-gcc')
env.Replace(OBJCOPY= 'avr-objcopy')
env.Replace(PROGSUFFIX='.elf')

mcu = 'atmega32u4'
f_cpu = 16000000
cstandard = 'gnu99'

env.Replace(CCFLAGS=['-mmcu={}'.format(mcu),
                     '-Os',
                     '-gdwarf-2',
                     '-DF_CPU={}UL'.format(f_cpu),
                     '-std={}'.format(cstandard),
                     '-funsigned-char',
                     '-funsigned-bitfields',
                     '-fpack-struct',
                     '-fshort-enums',
                     '-Wall',
                     '-Wa,-adhlns=${TARGET.dir}/${SOURCE.filebase}.lst',
                     '-Wstrict-prototypes'])

env.Replace(LINKFLAGS=[env['CCFLAGS'], '-Wl,--gc-sections,--relax'])

elf = SConscript('src/SConscript', variant_dir='build', duplicate=0)

# Generate binary files using OBJCOPY
hex=env.Command('main.hex',elf,'$OBJCOPY -O ihex -R .eeprom -R .fuse -R .lock $SOURCE $TARGET')
bin=env.Command('main.bin',elf,'$OBJCOPY -O binary -R .eeprom -R .fuse -R .lock $SOURCE $TARGET')

# Add binaries to list of default targets
Default(hex, bin)

# Programmming commands (binary and hex versions)
env.Command('programbin',bin,'avrdude -p atmega32u4 -P usb -c avrispv2 -U flash:w:$SOURCE')
env.Command('programhex',hex,'avrdude -p atmega32u4 -P usb -c avrispv2 -U flash:w:$SOURCE')
