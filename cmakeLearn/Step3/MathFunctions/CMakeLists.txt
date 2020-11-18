#库的编译  add_library() 默认编译静态库，如果编译动态库使用：
#add_library（MathFunctions SHARED mysqrt.cxx）
#add_library 还可以选择MODULE OBJECT 各个参数的含义
#参考 https://cmake.org/cmake/help/v3.0/manual/cmake-buildsystem.7.html#binary-library-types
add_library(MathFunctions mysqrt.cxx)