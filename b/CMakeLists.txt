cmake_minimum_required(VERSION 3.15)
project(MathFunctions-test)

find_package(MathFunctions REQUIRED)
find_package(tinyxml REQUIRED)

include(CTest)
add_executable(test-sqrt test-sqrt.cpp)
target_link_libraries(test-sqrt PRIVATE MathFunctions::MathFunctions cmeel::tinyxml)
add_test(NAME test-sqrt COMMAND test-sqrt)
