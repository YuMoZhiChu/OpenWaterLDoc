# OpenGL 搭建环境

### 安装和下载
我们需要一些工具和库来进行环境搭建,这些内容可以在 download 文件夹下找到
 >* cmake
 >* glfw
 >* glad
 >* pandoc

**cmake**
>* 环境搭建工具，我们使用它来构建我们的工程

**glfw**
>* openGL库

**glad**
>* openGL接口

**pandoc**
>* md文件转换工具

### 文件结构
初步阶段，只有三个文件夹
>* doc
>* src
>* external

**doc**
>* md 文件管理

**src**
>* 代码文件管理

**external**
>* 外部库管理，目前只有 glfw , glad

### 用CMake来构建项目
我们用cmake来搭建整个项目（使用VS2017）

>* 设置最小版本和项目名
``` c
cmake_minimum_required(VERSION 2.8)
cmake_policy(VERSION 2.8)

project(OpenWaterL)
```

>* 增加C++11特性
``` c
list(APPEND CMAKE_CXX_FLAGS "-std=c++11")
```

>* 设置最小版本和项目名
``` c
cmake_minimum_required(VERSION 2.8)
cmake_policy(VERSION 2.8)
```

>* debug/release 环境设置
>* 这里的set就是设置变量的意思，具体查询cmake api
``` c
set(CMAKE_DEBUG_POSTFIX "_d" CACHE STRING "add a postfix, usually d on windows")  
set(CMAKE_RELEASE_POSTFIX "" CACHE STRING "add a postfix, usually empty on windows")  

set(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/libs)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)  
```

>* 设置链接器寻找libs的目录
``` c
link_directories(${LIBRARY_OUTPUT_PATH})
```

>* 创建glfw
``` c
# first create relevant static libraries requried for other projects
# add glfw lib
set(GLFW_SOURCE "${CMAKE_SOURCE_DIR}/external/glfw/")
add_subdirectory(${GLFW_SOURCE} glfw3)
include_directories(${GLFW_SOURCE}/include)
# link different lib according to different build type
set(GLFW3 optimized glfw3 debug "glfw3${CMAKE_DEBUG_POSTFIX}")
```

>* 创建glad
``` c
set(GLAD_SOURCE "${CMAKE_SOURCE_DIR}/external/glad/")
add_subdirectory(${GLAD_SOURCE} glad)
include_directories(${GLAD_SOURCE}/include)
set(GLAD optimized glad debug "glad${CMAKE_DEBUG_POSTFIX}")
```

>* 设置libs 和 打印查看
>* 这里打印出来的libs是一大串字符，但其实是一个列表，opengl32表示的是3.2，不是32位
``` c
set(LIBS ${GLFW3} ${GLAD} opengl32)
message(STATUS, ${LIBS}, ${GLAD_SOURCE})
```

>* 搜索相应的文件，并加入项目
``` c
file(GLOB_RECURSE CPPFILE
	"src/code/*.cpp"
)

file(GLOB_RECURSE HEADFILE
	"src/code/*.h"
)

# 接入 md 文件
file(GLOB_RECURSE MARKDOWNFILE
	"doc/*.md"
)

message(STATUS, ${CPPFILE}, " 所有 CPP 文件")
message(STATUS, ${HEADFILE}, " 所有 H 文件")
add_executable(OpenWaterL ${CPPFILE} ${HEADFILE} ${MARKDOWNFILE})
```

>* 设置文件，md文件不参与编译，不加入编译过程
``` c
set(all_files ${HEADFILE} ${CPPFILE})
```

>* 这是在网上找到的一个marco，其实可以理解为一个函数，作用是根据文件夹的格式设置vs sln中的 filter
``` c
# 一个根据文件夹来进行分类的函数
macro(source_group_by_dir source_files)
    if(MSVC)
        set(sgbd_cur_dir ${CMAKE_CURRENT_SOURCE_DIR})
        foreach(sgbd_file ${${source_files}})
            string(REGEX REPLACE ${sgbd_cur_dir}/\(.*\) \\1 sgbd_fpath ${sgbd_file})
            string(REGEX REPLACE "\(.*\)/.*" \\1 sgbd_group_name ${sgbd_fpath})
            string(COMPARE EQUAL ${sgbd_fpath} ${sgbd_group_name} sgbd_nogroup)
            string(REPLACE "/" "\\" sgbd_group_name ${sgbd_group_name})
            if(sgbd_nogroup)
                set(sgbd_group_name "\\")
            endif(sgbd_nogroup)
            source_group(${sgbd_group_name} FILES ${sgbd_file})
        endforeach(sgbd_file)
    endif(MSVC)
endmacro(source_group_by_dir)
source_group_by_dir(all_files)
```

>* 把md文件单独独立出来
``` c
message(STATUS, ${MARKDOWNFILE}, " 所有 md 文件")
#设置 vs sln filter的接口
source_group("_markdownDoc" FILES ${MARKDOWNFILE})
```

>* 设置libs和依赖
>* 据朝通哥哥的说法，glad不知道为什么就已经依赖好了，当然，加上也没关系，不过会变慢
``` c
target_link_libraries(OpenWaterL ${LIBS})
add_dependencies(OpenWaterL glfw)
# add_dependencies(OpenWaterL glad)
```

>* 命令行执行cmake即可设置链接，并生成sln，用sln编译
>* 推荐加上 -A x64 生成64位的
``` c
cmake -A x64 CMakeLists.txt
```



