PART = "STM32F429"

CC = "arm-none-eabi-gcc"
AR = "arm-none-eabi-ar"

CMSIS = "../STM32CubeF4/Drivers/CMSIS"
HAL   = "../STM32CubeF4/Drivers/STM32F4xx_HAL_Driver"
BSP   = "../STM32CubeF4/Drivers/BSP/STM32F429I-Discovery"
directory BUILD = 'build'

CFLAGS = "-D #{PART}xx -D USE_HAL_DRIVER -Wall " \
    "-Os -nostdlib -fsigned-char -fno-inline -ffunction-sections " \
    "-mthumb -mlittle-endian -mcpu=cortex-m4 -mfloat-abi=hard -mfpu=fpv4-sp-d16 "\
    "-I./ -I#{HAL}/Inc -I#{CMSIS}/Include -I#{CMSIS}/Device/ST/STM32F4xx/Include "\
    "-I#{BSP}"


# ---------- library common routine ----------
def libraryTask(output,input,taskSym)
    object=[]
    FileList["#{input}/*.c"].each do |src|
        object << "#{BUILD}/#{src.pathmap("%n")}.o"
    end
    raise "No #{input} sources found for #{output}." unless not object.empty?
    task taskSym => output
    file output => [BUILD,*object] do
        puts "Building #{output} from #{input}"
        object.each do |obj|
            sh "#{AR} -r #{output} #{obj}"
        end
        sh "#{AR} -s #{output}"
    end
    rule '.o' => ->(out){"#{input}/#{out.pathmap("%n")}.c"} do |obj|
        sh "#{CC} #{CFLAGS} -c -o #{obj.name} #{obj.source}"
    end
end


# ---------- libraries for cpu and kit ----------
desc "Build stm32f429 cpu hal library"
libraryTask("#{BUILD}/libstm32f4hal.a", "#{HAL}/Src", :hal)

desc "Build stm32f429i-disco kit bsp library"
libraryTask("#{BUILD}/libstm32f4bsp.a", "#{BSP}", :bsp)


# ---------- mruby library ----------
MRUBY = "../mruby"
raise "No #{MRUBY} directory found." unless File.exist?("#{MRUBY}")
desc "Build mruby library"
task :mruby => BUILD do
    Dir.chdir("#{MRUBY}") do
        sh 'rake MRUBY_CONFIG="../mruby-cube/build_config_mruby.rb"'
    end
    cp "#{MRUBY}/build/#{PART}/lib/libmruby.a", "#{BUILD}"
end


# ---------- clean up before a rebuild ----------
desc "Delete build outputs"
task :clean do
    rm_rf "#{BUILD}"
    Dir.chdir("#{MRUBY}") do
        sh 'rake clean'
    end
end


# ---------- default build everything ----------
desc "Build all"
task :default =>[:hal, :bsp, :mruby]

