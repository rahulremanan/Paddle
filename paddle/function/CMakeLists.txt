file(GLOB h_files . *Op.h)
file(GLOB cpp_files . *Op.cpp)

list(APPEND h_files Function.h)
list(APPEND cpp_files Function.cpp)
list(APPEND cpp_files BufferArg.cpp)

if(WITH_GPU)
    file(GLOB cu_files . *OpGpu.cu)
    cuda_compile(cu_objs ${cu_files})
endif()

add_library(paddle_function STATIC ${cpp_files} ${cu_objs})
add_dependencies(paddle_function ${external_project_dependencies})


if(WITH_GPU)
if(WITH_TESTING)
    # TODO:
    # file(GLOB test_files . *OpTest.cpp)
    # add_executable(${test_bin} EXCLUDE_FROM_ALL ${test_files})
    add_simple_unittest(CrossMapNormalOpTest)
    add_simple_unittest(TensorShapeTest)
    add_simple_unittest(TensorTypeTest)
    add_simple_unittest(BufferArgTest)
    add_simple_unittest(FunctionTest)
    add_simple_unittest(ContextProjectionOpTest)
endif()
endif()

add_style_check_target(paddle_function ${h_files})
add_style_check_target(paddle_function ${cpp_files})
if(WITH_GPU)
    add_style_check_target(paddle_function ${cu_files})
endif()
