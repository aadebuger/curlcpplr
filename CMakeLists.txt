project( curlexample1 )
cmake_minimum_required( VERSION 2.8 )

# remove compiler checks if you are using VS2010, it supports
#   1. auto
#   2. lambda
#   3. const iterator
# and, sadly, it doesn't support `for(item, cotainer)'
#  BOOST_ROOT_DIR=/usr/local/Cellar/boost/1.63.0/
include( CheckCXXCompilerFlag )
check_cxx_compiler_flag( "-std=c++11"   COMPILER_SUPPORTS_CXX11 )
check_cxx_compiler_flag( "-std=c++0x"   COMPILER_SUPPORTS_CXX0X )
if( COMPILER_SUPPORTS_CXX11 )
    if( CMAKE_COMPILER_IS_GNUCXX )
        set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11" )
    else()
        set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11" )
    endif()
elseif( COMPILER_SUPPORTS_CXX0X)
    set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x" )
else()
    # MSVC, On by default (if available)
endif()
ADD_SUBDIRECTORY(ext/curlcpp) # Change `ext/curlcpp` to a directory according to your setup
INCLUDE_DIRECTORIES(${CURLCPP_SOURCE_DIR}/include)
include("cmake/Modules/FindGlog.cmake")  
#find_package(lib_glog REQUIRED)  
find_package(CURL  REQUIRED)
include_directories(${CURL_INCLUDE_DIRS})

include_directories(${GLOG_INCLUDE_DIRS})

add_executable( ${PROJECT_NAME} main.cpp )

if(MSVC)
MESSAGE( STATUS "MSVC")
MESSAGE( STATUS  ${CURL_LIBRARY} )
set(CMAKE_CXX_FLAGS_RELEASE "/MT")
set(CMAKE_CXX_FLAGS_DEBUG "/MTd")
link_directories( ${CURL_LIBRARY} )
target_link_libraries(${PROJECT_NAME} curlcpp d:/libcurl.lib)
else()
target_link_libraries(${PROJECT_NAME} curlcpp)

endif()


target_link_libraries(${PROJECT_NAME} ${GLOG_LIBRARIES})

add_executable( curlstring mainstring.cpp )

target_link_libraries(curlstring curlcpp)

add_executable( threadfuture mainfuture.cpp )

target_link_libraries(threadfuture curlcpp)

#add_executable( cp11-async-curl cp11-async-curl.cpp )

#target_link_libraries(cp11-async-curl curlcpp)

#add_executable( boostsignals2 boost-signals2lr.cpp )

#target_link_libraries(cp11-async-curl )


