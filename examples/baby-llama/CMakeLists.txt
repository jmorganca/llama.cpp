set(TARGET llama-baby-llama)
add_executable(${TARGET} baby-llama.cpp)
install(TARGETS ${TARGET} RUNTIME)
target_link_libraries(${TARGET} PRIVATE common llama ${CMAKE_THREAD_LIBS_INIT})
target_compile_features(${TARGET} PRIVATE cxx_std_11)

# Additional configurations
# Set compiler warnings to treat as errors for better code quality
if(MSVC)
    target_compile_options(${TARGET} PRIVATE /W4 /WX)  # For MSVC
else()
    target_compile_options(${TARGET} PRIVATE -Wall -Werror)  # For GCC/Clang
endif()

# Include directories for header files
target_include_directories(${TARGET} PRIVATE ${CMAKE_SOURCE_DIR}/include)

# Optionally add another executable target
set(SECOND_TARGET llama-baby-llama-utils)
add_executable(${SECOND_TARGET} baby-llama-utils.cpp)
install(TARGETS ${SECOND_TARGET} RUNTIME)
target_link_libraries(${SECOND_TARGET} PRIVATE common llama ${CMAKE_THREAD_LIBS_INIT})
target_compile_features(${SECOND_TARGET} PRIVATE cxx_std_11)

# Optionally link additional libraries if needed
find_package(OpenMP REQUIRED)
target_link_libraries(${TARGET} PRIVATE OpenMP::OpenMP_CXX)
target_link_libraries(${SECOND_TARGET} PRIVATE OpenMP::OpenMP_CXX)
