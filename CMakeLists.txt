cmake_minimum_required(VERSION 3.12) # 最低的CMAKE版本
project(caffeReadImgClassify)
# 可以用这个指令定义工程的名称,并指定工程支持的语言,语言列表是可以忽略的,默认情况表示支持所有语言
# 这个指令还隐式定义了两个cmake变量 projectname_BINARY_DIR 和 projectname_SOURCE_DIR,因为采用的是内部编
#　译,两个变量目前指的是工作所在路径，如果是外部编译，两者所指代的内容会有所不同，但是如果改变工程名，
# 那么这两个变量也需要修改，那会很麻烦，所有，建议直接使用PROJECT_BINARY_DIR 和 PROJECT_SOURCE_DIR
# 外部编译时,PROJECT_BINATY_DIR指的是编译路径, PROJECT_SOURCE_DIR指工程路径

set(CMAKE_CXX_STANDARD 14)  #用来定义显式变量

include_directories(/home/hjxu/git/caffe/include
        /usr/local/cuda/include /usr/include/opencv
        /usr/include/boost
        /home/hjxu/git/caffe/.build_release/src)

# INCLUDE_DIRECTORIES([AFTER|BEFORE] [SYSTEM] dir1 dir2 ...)
# 这条指令可以用来向工程中添加多个特定的头文件搜索路径, 路径之间用空格分割,如果路径中包含了空格,
# 可以用双引号将它括起来, 默认的行为是追加到当前头文件搜索路径的后面,当然,也可以通过两种方式来控制搜索路径添加的方式
# 一种方式:CMAKE_INCLUDE_DIRECORIES_BEFORE,通过SET这个cmake变量为on,可以将添加的头文件搜索路径放在已有路径的前面
# 第二种方式: 通过AFTER 或者 BEFORE 参数控制是追加还是置前

find_library(caffe /home/hjxu/git/caffe/build/lib)
# find_library(<VAR> name1 path1 path2 ...)
#  VAR变量表示要找的库的全路径, 包含库文件名


link_libraries("/home/hjxu/git/caffe/.build_release/lib/libcaffe.so"
        "/usr/lib/x86_64-linux-gnu/libglog.so"
        "/usr/lib/x86_64-linux-gnu/libboost_filesystem.so"
        "/usr/lib/x86_64-linux-gnu/libboost_system.so")
# LINL_LIBRARIES 添加需要库链接的库文件路径,这里是全路径

find_package(OpenCV REQUIRED)
# FIND_PACKAGE(<name> [major.minor] [QUIET] [NO_MODULE] [[REQUIRED|COMPONENTS] [componets...]])
# QUIET 会执行 caffeReadImgClassify_FIND_QUIETLY,如果不指定这个参数,就会执行
# MESSAGE(STATUS "Found caffeReadImgClassify: &{caffeReadImgClassify_LIBRARY}")
# REQUIRED 参数, 其含义是这个共享库是否是工程必须的,如果使用了这个参数,说明这个链接库是必须库,
# 如果找不到这个链接库,则工程不能编译,对应于.cmake模块中的HELLO_FIND_REQUIRED 变量

link_directories("/usr/local/lib/")
# 添加非标准共享库的搜索路径, 如果,在工程内部同时存在共享库和可执行二进制, 在编译时就需要指定一下这些共享库的路径.

set(SOURCE_FILES main.cpp)

add_executable(caffeReadImgClassify ${SOURCE_FILES})
# cmake会自动的在本目录查找main.c或者main.cpp等,但是最好不要投这个懒,万一目录中有 main.

target_link_libraries(caffeReadImgClassify ${OpenCV_LIBS})
# TARGET_LINK_LIBRARIES(target library1 <debug | optimized> library2 ...)
# 这个指令可以用来为target添加需要链接的共享库,本例中是一个可执行文件,但是同样可以用于为自己编写的共享库
# 添加共享库链接