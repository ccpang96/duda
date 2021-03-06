cmake_minimum_required(VERSION 2.8)

project(web)

# 添加头文件路径
include_directories(
    "${PROJECT_SOURCE_DIR}/http"
    "${PROJECT_SOURCE_DIR}/log"
    "${PROJECT_SOURCE_DIR}/lock"
    "${PROJECT_SOURCE_DIR}/threadpool"
    "${PROJECT_SOURCE_DIR}/timer"
    "${PROJECT_SOURCE_DIR}/CGImysql"
    "${PROJECT_SOURCE_DIR}/config"



)

# 添加子目录
add_subdirectory(http)
add_subdirectory(log)
add_subdirectory(CGImysql)
add_subdirectory(timer)
add_subdirectory(config)

# 添加mysql库 pthread库
find_package (Threads)
find_package(PkgConfig)
pkg_check_modules(MySQL REQUIRED mysqlclient>=5.7)

add_executable(${PROJECT_NAME} "main.cpp" "webserver.cpp")
target_link_libraries(${PROJECT_NAME} http_function log_function cgimysql_function  config_function  timer_function)
target_link_libraries (${PROJECT_NAME}  ${CMAKE_THREAD_LIBS_INIT})
target_include_directories(${PROJECT_NAME} PUBLIC ${MySQL_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} PUBLIC ${MySQL_LIBRARIES})
