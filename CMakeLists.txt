cmake_minimum_required(VERSION 3.0)
project(TinyCC
	VERSION 0.9.27
	# DESCRIPTION "Tiny C Compiler" # cmake version >= 3.9
	# HOMEPAGE_URL "www.zhyingkun.com"
	LANGUAGES C # 移除CXX，TCC是纯C项目，减少不必要依赖
)

message(STATUS "CMakeLists.txt for ${PROJECT_NAME}")
message(STATUS "Current OS is ${CMAKE_SYSTEM}")
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(CMAKE_SKIP_BUILD_RPATH FALSE)
set(CMAKE_SKIP_INSTALL_RPATH FALSE)
set(CMAKE_BUILD_WITH_INSTALL_RPATH FALSE)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH FALSE) # 修复拼写错误：FLASE -> FALSE
# set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib") # must before add_executable

# 核心修改：添加静态库/动态库控制变量，支持CMake GUI配置
# 1. 定义BUILD_SHARED_LIBS变量，默认值OFF（默认生成静态库）
# 2. 在CMake GUI中可可视化勾选/取消，无需手动修改代码
option(BUILD_SHARED_LIBS "Build shared libraries (DLL) instead of static libraries (LIB)" OFF)

# 验证变量配置（可选，方便在终端查看当前库类型）
if(BUILD_SHARED_LIBS)
    message(STATUS "Configured to build SHARED library (DLL)")
else()
    message(STATUS "Configured to build STATIC library (LIB)")
endif()

add_subdirectory(libtcc) # 核心库模块，会自动遵循BUILD_SHARED_LIBS配置
add_subdirectory(tcc)     # 可执行文件模块，依赖libtcc
if(WIN32)
else(WIN32)
	add_subdirectory(runtime)
endif(WIN32)
add_subdirectory(demo)    # 示例程序模块
