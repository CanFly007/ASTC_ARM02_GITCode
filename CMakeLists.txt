cmake_minimum_required(VERSION 3.10) # 指定最低CMake版本

# 平台特定配置
if(APPLE)
    # 设置 macOS 的目标架构为 x86_64 和 arm64
    set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64" CACHE STRING "Build architectures for Mac OS X" FORCE)
elseif(WIN32)
    # Windows 平台特定的配置，如果有的话
    # 例如，你可能需要设置用于 Windows 的编译器标志或定义
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(astc_arm) # 项目名称

# 指定生成的库类型为DLL
set(BUILD_SHARED_LIBS ON)

# 收集源文件和头文件
file(GLOB_RECURSE SOURCES Source/*.cpp)
file(GLOB_RECURSE HEADERS Source/*.h)

# 创建动态链接库目标
add_library(astc_arm SHARED ${SOURCES} ${HEADERS})

# 将DLL导出宏定义添加到预处理器
target_compile_definitions(astc_arm PRIVATE ASTCENC_DYNAMIC_LIBRARY)

# 使用 PRIVATE 是因为您不希望这些头文件路径在您的库之外被暴露，从而避免可能的命名冲突。
target_include_directories(astc_arm PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/Source/ThirdParty)

# 指定包含目录（对外提供头文件）
target_include_directories(astc_arm PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/Source)



# 配置安装规则，可选
# install(TARGETS astc_arm
#     RUNTIME DESTINATION bin
#     LIBRARY DESTINATION lib
#     ARCHIVE DESTINATION lib)
# install(FILES ${HEADERS} DESTINATION include)