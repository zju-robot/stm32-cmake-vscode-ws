cmake_minimum_required(VERSION 3.22)

project(stm32-cmake-vscode)
enable_language(C ASM)

# # # # # # # # # #
# 源文件、头文件设置
# # # # # # # # # #

# 源文件列表
# set(STARTUP_SRC startup_stm32f303x8.s)
# aux_source_directory(Core/Src CORE_SRCS)
# aux_source_directory(Drivers/STM32F3xx_HAL_Driver/Src HAL_DRIVERS_SRCS)
set(SOURCES ${STARTUP_SRC} ${CORE_SRCS} ${HAL_DRIVERS_SRCS})

# 头文件搜索目录
#set(INCLUDE_DIRECTORIES
#    Core/Inc
#    Drivers/STM32F3xx_HAL_Driver/Inc
#    Drivers/STM32F3xx_HAL_Driver/Inc/Legacy
#    Drivers/CMSIS/Device/ST/STM32F3xx/Include
#    Drivers/CMSIS/Include)

# 链接文件
# set(LD_SCRIPT ../STM32F303K8Tx_FLASH.ld)


# # # # # # # # # #
# 编译、链接选项
# # # # # # # # # #

# MCU相关选项
# add_compile_options(-mcpu=cortex-m4 -mthumb)
# add_link_options(-mcpu=cortex-m4 -mthumb)

# FPU相关选项
# add_compile_definitions(ARM_MATH_CM4 ARM_MATH_MATRIX_CHECK ARM_MATH_ROUNDING)
# add_compile_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
# add_link_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)

# Warnings相关选项
add_compile_options(-Wall -Wextra -Wpedantic -Wno-unused-parameter)

# 全局宏定义
# add_compile_definitions(USE_HAL_DRIVER STM32F303x8)

# 分区选项
add_compile_options(-ffunction-sections -fdata-sections -fno-common)
add_link_options(-Wl,-gc-sections)

# 输出选项
add_compile_options(-fmessage-length=0)
add_link_options(-Wl,--print-memory-usage)

# 指定链接文件
add_link_options(-T ${LD_SCRIPT})

# 包含头文件
include_directories(${INCLUDE_DIRECTORIES})


# # # # # # # # # #
# 编译目标设置
# # # # # # # # # #

# 目标名称
set(TARGET firmware)

# 添加目标及其相关配置
add_executable(${TARGET} ${SOURCES} ${LD_SCRIPT})

# 生成纯二进制文件
add_custom_command(
  TARGET ${TARGET}
  POST_BUILD
  COMMAND ${CMAKE_OBJCOPY} -O binary ${TARGET}.elf ${TARGET}.bin)
