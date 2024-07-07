cmake_minimum_required (VERSION 3.8)


#Parametry konfiguracyjne platformy programistycznej i flagi kompilatora
set(CMAKE_CXX_COMPILER g++)#g++ jako kompilator C++
set(CMAKE_C_COMPILER gcc)#gcc jako kompilator C
set(CMAKE_CXX_STANDARD 11)#Standard C++ 11, bez tego parametru g++ użyje wartości domyślnej. Dla g++ od wersji 6 jest to C++14
set(CMAKE_CXX_STANDARD_REQUIRED ON)#Określony wyżej standard C++ jest wymagany

#Profil debug i maksymalny poziom debug info g3 oraz format debug info na dwarf-4 (musi być gdb w wersji 7). BUILD_TYPE może być ustawiony na:
#empty
#Debug
#Release
#RelWithDebInfo
#MinSizeRel
#Ustawiono dodatkowo flagi debug generujące informację dla debuggera
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_C_FLAGS_DEBUG "-g3 -gdwarf-4")
set(CMAKE_CXX_FLAGS_DEBUG "-g3 -gdwarf-4")
#Ustaw nazwę projektu i język na C++
project(ImGui)
enable_language(CXX)
#W specjalnym pliku compile_commands.json można podejrzeć wszystkie operacje kompilatora wraz z parametrami,
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_VERBOSE_MAKEFILE TRUE)
#Uwzględnij podprojekt znajdujący się w podkatalogu program. Musi być w nim pliki CMakeLists.txt
#Włączenie testów powoduje pojawienie się celu test w makefiles
enable_testing()

include_directories(src)
include_directories(imgui)
include_directories(imgui/backends)
link_directories(/lib64)

set(SOURCE_FILES
    src/builder.cpp
    src/builder.h
    imgui/imgui.cpp
    imgui/imgui.h
    imgui/imgui_demo.cpp
    imgui/imgui_draw.cpp
    imgui/imgui_internal.h
    imgui/imgui_widgets.cpp
    imgui/imgui_tables.cpp
    imgui/backends/imgui_impl_glfw.cpp
    imgui/backends/imgui_impl_opengl3.cpp
    imgui/backends/imgui_impl_glfw.h
    imgui/backends/imgui_impl_opengl3.h
    imgui/misc/cpp/imgui_stdlib.cpp
    imgui/misc/cpp/imgui_stdlib.h
)

add_library(Library ${SOURCE_FILES})

target_include_directories(Library PUBLIC src)


find_package(glfw3 REQUIRED)
target_link_libraries(Library PUBLIC glfw)
find_package(OpenGL REQUIRED)
target_link_libraries(Library PUBLIC ${OPENGL_LIBRARIES})

add_executable(Program src/main.cpp)
target_link_libraries(Program Library)
