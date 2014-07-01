PART = "STM32F429"

CC = "arm-none-eabi-gcc"
AR = "arm-none-eabi-ar"

CMSIS = "../STM32Cube_FW_F4/Drivers/CMSIS"
HAL   = "../STM32Cube_FW_F4/Drivers/STM32F4xx_HAL_Driver"

CFLAGS = "-D #{PART}xx -D USE_HAL_DRIVER -Wall " \
    "-Os -nostdlib -fsigned-char -fno-inline -ffunction-sections " \
    "-mthumb -mlittle-endian -mcpu=cortex-m4 -mfloat-abi=hard -mfpu=fpv4-sp-d16 "\
    "-I./ -I#{HAL}/Inc -I#{CMSIS}/Include -I#{CMSIS}/Device/ST/STM32F4xx/Include"

directory BUILD = 'Build'
LIB   = "#{BUILD}/libSTM32F4hal.a"
object=[]
FileList["#{HAL}/Src/*.c"].each do |src|
    object << "#{BUILD}/#{src.pathmap("%n")}.o"
end

desc "Make cube hal library"
task :default => [BUILD,LIB]

file LIB => object do
    sh "#{AR} -rs #{LIB} #{BUILD}/*.o"
end

rule '.o' => ->(out){"#{HAL}/Src/#{out.pathmap("%n")}.c"} do |obj|
    sh "#{CC} #{CFLAGS} -c -o #{obj.name} #{obj.source}"
end

desc "Clean deletes build directory"
task :clean do
    sh "rm -Rf #{BUILD}"
end

desc "Build mruby"
task :mruby do
    Dir.chdir('../mruby') do
        sh 'rake MRUBY_CONFIG="../mruby-cube/build_config_mruby.rb"'
    end
end

desc "Clean lightly mruby"
task :clean_mruby do
    Dir.chdir('../mruby') do
        sh 'rake clean'
    end
end
