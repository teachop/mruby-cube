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

desc "Build all"
task :default =>[:hal, :bsp, :mruby]


# ---------- build stmicro hal library for cpu ----------
begin
    LIB_HAL   = "#{BUILD}/libstm32f4hal.a"
    objhal=[]
    FileList["#{HAL}/Src/*.c"].each do |src|
        objhal << "#{BUILD}/#{src.pathmap("%n")}.o"
    end
    raise "No #{HAL} sources found." unless not objhal.empty?
    desc "Build stm32cube hal library"
    task :hal => LIB_HAL
    file LIB_HAL => [BUILD,*objhal] do
        objhal.each do |obj|
            sh "#{AR} -r #{LIB_HAL} #{obj}"
        end
        sh "#{AR} -s #{LIB_HAL}"
    end
    rule '.o' => ->(out){"#{HAL}/Src/#{out.pathmap("%n")}.c"} do |obj|
        sh "#{CC} #{CFLAGS} -c -o #{obj.name} #{obj.source}"
    end
end


# ---------- build stmicro bsp library for kit ----------
begin
    LIB_BSP   = "#{BUILD}/libstm32f4bsp.a"
    objbsp=[]
    FileList["#{BSP}/*.c"].each do |src|
        objbsp << "#{BUILD}/#{src.pathmap("%n")}.o"
    end
    raise "No #{BSP} sources found." unless not objbsp.empty?
    desc "Build stm32f429i kit bsp library"
    task :bsp => LIB_BSP
    file LIB_BSP => [BUILD,*objbsp] do
        objbsp.each do |obj|
            sh "#{AR} -r #{LIB_BSP} #{obj}"
        end
        sh "#{AR} -s #{LIB_BSP}"
    end
    rule '.o' => ->(out){"#{BSP}/#{out.pathmap("%n")}.c"} do |obj|
        sh "#{CC} #{CFLAGS} -c -o #{obj.name} #{obj.source}"
    end
end


# ---------- build mruby library ----------
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
