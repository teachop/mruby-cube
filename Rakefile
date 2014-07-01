PART = "STM32F429"

CC = "arm-none-eabi-gcc"
AR = "arm-none-eabi-ar"

CMSIS = "../STM32Cube_FW_F4/Drivers/CMSIS"
HAL   = "../STM32Cube_FW_F4/Drivers/STM32F4xx_HAL_Driver"

CFLAGS = "-D #{PART}xx -D USE_HAL_DRIVER -Wall " \
    "-Os -nostdlib -fsigned-char -fno-inline -ffunction-sections " \
    "-mthumb -mlittle-endian -mcpu=cortex-m4 -mfloat-abi=hard -mfpu=fpv4-sp-d16 "\
    "-I./ -I#{HAL}/Inc -I#{CMSIS}/Include -I#{CMSIS}/Device/ST/STM32F4xx/Include"

directory BUILD = 'build'
LIB   = "#{BUILD}/libSTM32F4hal.a"
object=[]
FileList["#{HAL}/Src/*.c"].each do |src|
    object << "#{BUILD}/#{src.pathmap("%n")}.o"
end

desc "Build all"
task :default =>[:hal, :mruby]

desc "Build stm32cube hal library"
task :hal => [BUILD,LIB]

file LIB => object do
    sh "#{AR} -rs #{LIB} #{BUILD}/*.o"
end

rule '.o' => ->(out){"#{HAL}/Src/#{out.pathmap("%n")}.c"} do |obj|
    sh "#{CC} #{CFLAGS} -c -o #{obj.name} #{obj.source}"
end

desc "Clean all"
task :clean => [:clean_build, :clean_mruby]

desc "Deletes build output directory"
task :clean_build do
    sh "rm -Rf #{BUILD}"
end

desc "Build mruby library"
task :mruby do
    Dir.chdir('../mruby') do
        sh 'rake MRUBY_CONFIG="../mruby-cube/build_config_mruby.rb"'
        sh "mkdir -p ../mruby-cube/#{BUILD}"
        sh "cp build/#{PART}/lib/libmruby.a ../mruby-cube/#{BUILD}"
    end
end

desc "Clean lightly mruby"
task :clean_mruby do
    Dir.chdir('../mruby') do
        sh 'rake clean'
    end
end
