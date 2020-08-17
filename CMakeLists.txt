cmake_minimum_required(VERSION 3.5)

if(NOT DEFINED ENV{TRAVIS_BUILD_NUMBER})
    set(ENV{TRAVIS_BUILD_NUMBER} 1)
endif()

project(bulklib VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})

find_package(Threads)

add_executable(async observer.h observer.cpp main.cpp async.cpp async.h)

set_target_properties(async PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    COMPILE_OPTIONS "-Wall;-Wpedantic;-Wextra;" #-fsanitize=thread
#    LINK_OPTIONS "-fsanitize=thread"
)

target_link_libraries(async
    ${CMAKE_THREAD_LIBS_INIT}
)

install(TARGETS async RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT "HankHenshaw at github.com")

include(CPack)