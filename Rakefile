PART = "STM32F429"

CC = "arm-none-eabi-gcc"
AR = "arm-none-eabi-ar"

CMSIS = "../STM32CubeF4/Drivers/CMSIS"
HAL   = "../STM32CubeF4/Drivers/STM32F4xx_HAL_Driver"
directory BUILD = 'build'
LIB   = "#{BUILD}/libSTM32F4hal.a"

CFLAGS = "-D #{PART}xx -D USE_HAL_DRIVER -Wall " \
    "-Os -nostdlib -fsigned-char -fno-inline -ffunction-sections " \
    "-mthumb -mlittle-endian -mcpu=cortex-m4 -mfloat-abi=hard -mfpu=fpv4-sp-d16 "\
    "-I./ -I#{HAL}/Inc -I#{CMSIS}/Include -I#{CMSIS}/Device/ST/STM32F4xx/Include"

desc "Build all"
task :default =>[:hal, :mruby]

# ---------- create a list of the hal library object files ----------
object=[]
FileList["#{HAL}/Src/*.c"].each do |src|
    object << "#{BUILD}/#{src.pathmap("%n")}.o"
end
if object.empty? then
    puts "No hal library sources found."
    exit
end

# ---------- build stmicro hal library for cpu ----------
desc "Build stm32cube hal library"
task :hal => LIB
file LIB => [BUILD,*object] do
    sh "#{AR} -rs #{LIB} #{BUILD}/*.o"
end
rule '.o' => ->(out){"#{HAL}/Src/#{out.pathmap("%n")}.c"} do |obj|
    sh "#{CC} #{CFLAGS} -c -o #{obj.name} #{obj.source}"
end

# ---------- build mruby as a library ----------
MRUBY = "../mruby"
if not File.exists?("#{MRUBY}") then
    puts "No mruby project directory found."
    exit
end
desc "Build mruby library"
task :mruby => BUILD do
    Dir.chdir("#{MRUBY}") do
        sh 'rake MRUBY_CONFIG="../mruby-cube/build_config_mruby.rb"'
    end
    cp "#{MRUBY}/build/#{PART}/lib/libmruby.a", "#{BUILD}"
end

# ---------- clean up for a rebuild ----------
desc "Delete build outputs"
task :clean do
    rm_rf "#{BUILD}"
    Dir.chdir("#{MRUBY}") do
        sh 'rake clean'
    end
end
