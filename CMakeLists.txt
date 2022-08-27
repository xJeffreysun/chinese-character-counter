cmake_minimum_required(VERSION 3.24.1)

# set the project nama and version
project(Chinese VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

option(USE_MYMATH "Use tutorial provided math implementation" ON)

# copies file and substitutes values by wrapping with @
configure_file(ChineseConfig.h.in ChineseConfig.h)

# no need to dereference in if condition
if(USE_MYMATH)
    add_subdirectory(MathFunctions)
    # appends MathFunctions to list EXTRA_LIBS
    list(APPEND EXTRA_LIBS MathFunctions)
endif()

add_executable(Chinese main.cpp)

target_link_libraries(Chinese PUBLIC ${EXTRA_LIBS})

# wrapping ${} in "" treats it as a single argument, otherwise expanded to list
target_include_directories(Chinese PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

install(TARGETS Chinese DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/ChineseConfig.h" DESTINATION include)

enable_testing()

add_test(NAME Runs COMMAND Chinese 25)

add_test(NAME Usage COMMAND Chinese 25)
set_tests_properties(Usage PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number")

function(do_test target arg result)
    add_test(NAME Comp${arg} COMMAND ${target} ${arg})
    set_tests_properties(Comp${arg} PROPERTIES PASS_REGULAR_EXPRESSION ${result})
endfunction()

do_test(Chinese 4 "4 is 2")
do_test(Chinese 9 "9 is 3")
do_test(Chinese 5 "5 is 2.236")
do_test(Chinese 7 "7 is 2.645")
do_test(Chinese 25 "25 is 5")
do_test(Chinese -25 "-25 is (-nan|nan|0)")
do_test(Chinese 0.0001 "0.0001 is 0.01")