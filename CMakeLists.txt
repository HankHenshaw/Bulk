cmake_minimum_required(VERSION 3.5)

if(NOT DEFINED ENV{TRAVIS_BUILD_NUMBER})
    set(ENV{TRAVIS_BUILD_NUMBER} 1)
endif()

project(bulklib VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})

find_package(Threads)

set(LIBFILES async.h async.cpp observer.h observer.cpp)

add_library(async SHARED ${LIBFILES})
add_executable(async_cli main.cpp)

set_target_properties(async_cli PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    COMPILE_OPTIONS "-Wall;-Wpedantic;-Wextra;"
)

target_link_libraries(async_cli
    ${CMAKE_THREAD_LIBS_INIT}
    async
)

install(TARGETS async_cli RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT "HankHenshaw at github.com")

include(CPack)