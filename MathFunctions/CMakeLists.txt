add_library(MathFunctions mysqrt.cpp)

# anybody linking to MathFunctions needs to include current source directory
target_include_directories(MathFunctions INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)