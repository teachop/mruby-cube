PART = "STM32F429"

CC = "arm-none-eabi-gcc"
AR = "arm-none-eabi-ar"

CMSIS = "../STM32Cube_FW_F4/Drivers/CMSIS"
HAL   = "../STM32Cube_FW_F4/Drivers/STM32F4xx_HAL_Driver"

LIB   = 'libSTM32F4_CUBE.a'

CFLAGS = "-D #{PART}xx -D USE_HAL_DRIVER -Wall " \
    "-Os -nostdlib -fsigned-char -fno-inline -ffunction-sections " \
    "-mthumb -mlittle-endian -mcpu=cortex-m4 -mfloat-abi=hard -mfpu=fpv4-sp-d16 "\
    "-I./ -I#{HAL}/Inc -I#{CMSIS}/Include -I#{CMSIS}/Device/ST/STM32F4xx/Include"

FileList["#{HAL}/Src/*.c"].each do |src|
    file LIB => src.ext('.o')
end

desc "Make cube hal library"
task :default => LIB do
    sh "#{AR} s #{LIB}"
end

desc "Clean cube hal library build artifacts"
task :clean do
    sh "rm -f #{LIB}"
    sh "rm -f #{HAL}/Src/*.o"
end

rule '.o' => '.c' do |obj|
    sh "#{CC} #{CFLAGS} -c -o #{obj} #{obj.name().ext('.c')}"
    sh "#{AR} -r #{LIB} #{obj}"
end
