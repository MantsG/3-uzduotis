cmake_minimum_required(VERSION 3.14)
project(studentu_rusiavimas LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# --- Core biblioteka iš tavo .cpp failų ---
add_library(core
  Studentas.cpp
  Utils.cpp
  Ivedimas.cpp
  Failai.cpp
)

# Kad #include "Studentas.h" ir pan. veiktų
target_include_directories(core PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# Optimizacija (vietoj global CMAKE_CXX_FLAGS)
target_compile_options(core PRIVATE -O1)

# --- Pagrindinis vykdomasis (main.cpp) ---
if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/main.cpp")
  add_executable(studentai main.cpp)
elseif (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp")
  add_executable(studentai src/main.cpp)
  target_include_directories(studentai PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
else()
  message(WARNING "Nerastas main.cpp (nei šaknyje, nei src/main.cpp). Bus galima kompiliuoti tik tests targetą.")
endif()

if (TARGET studentai)
  target_link_libraries(studentai PRIVATE core)
  target_compile_options(studentai PRIVATE -O1)
endif()
