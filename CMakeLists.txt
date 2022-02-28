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
set(MCU_FLAGS -mthumb -mcpu=cortex-m4 -mfpu=fpv4-sp-d16 -mfloat-abi=hard)

# Warnings相关选项
set(WARNING_FLAGS -Wall -Wextra -Wpedantic -Wno-unused-parameter)

# 全局宏定义
# set(DEFINITIONS -DUSE_HAL_DRIVER -DSTM32F303x8)

# 生成编译链接选项
set(COMPILE_FLAGS
    ${MCU_FLAGS} ${WARNING_FLAGS}
    -nostdlib  -fdata-sections -ffunction-sections
)
set(LINK_FLAGS
    ${MCU_FLAGS} -T${LD_SCRIPT}
    -lc -lm -lnosys -Wl,--gc-sections
    -Wl,--print-memory-usage
)


# # # # # # # # # #
# 编译目标设置
# # # # # # # # # #

# 目标名称
set(TARGET firmware)

# 添加目标及其相关配置
add_executable(${TARGET} ${SOURCES})
target_include_directories(${TARGET} PRIVATE ${INCLUDE_DIRECTORIES})
target_compile_definitions(${TARGET} PRIVATE ${DEFINITIONS})
target_compile_options(${TARGET} PRIVATE ${COMPILE_FLAGS})
target_link_options(${TARGET} PRIVATE ${LINK_FLAGS})

# 生成纯二进制文件
add_custom_command(
  TARGET ${TARGET}
  POST_BUILD
  COMMAND ${CMAKE_OBJCOPY} -O binary ${TARGET}.elf ${TARGET}.bin)
