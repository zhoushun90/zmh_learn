#学习点
#1.cmake添加动态库
#2.宏定义的使用
#3.配置头文件的使用
#4.编译库 add_library()


cmake_minimum_required(VERSION 3.1.2)

#设置项目名称和版本
project(Tutorial VERSION 2.0)

#指定C++标准( cmake 3.1版本后才有的特性)
set(CMAKE_CXX_STANDARD 11)
SET(CMAKE_CXX_STANDARD_REQUIRED TRUE)

#判断编译器是否支持C++11
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
        message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()


# 配置头文件传递CMake 的一些设置到源码文件,注意configure_file() 命令默认将配置文件放到PROJECT_BINARY_DIR目录下
configure_file(TutorialConfig.h.in TutorialConfig.h)



#判断是否使用自己的数学函数库
option(USE_MYMATH "use tutorial provided math implementation" ON)

#添加 MathFunctions 库
if(USE_MYMATH)
add_subdirectory(MathFunctions)
list(APPEND EXTRA_LIBS MathFunctions)
list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()

#添加可执行文件  #就是常见的main.cpp
add_executable(Tutorial tutorial.cxx)

#指定第三方库
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

#添加构建目录树到头文件的搜索路径，这样就可以找到配置项头文件TutorialConfig.h
target_include_directories(Tutorial PUBLIC  ${PROJECT_BINARY_DIR} ${EXTRA_INCLUDES})

#debug模式下,加-Os，速度会快
#Release模式下，加 -O2,和不加，有比较显著的差异