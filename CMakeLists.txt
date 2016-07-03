cmake_minimum_required(VERSION 3.2)

project(sqlite LANGUAGES C)

set(sqlite_VERSION 3.13.0)

if(MSVC)
  set(CMAKE_DEBUG_POSTFIX "d")
endif()


add_library(sqlite STATIC
  sqlite3.h
  sqlite3ext.h
  sqlite3.c
)
target_include_directories(
  sqlite PRIVATE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  INTERFACE
  $<INSTALL_INTERFACE:include>
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/sqlite-config-version.cmake
  VERSION ${sqlite_VERSION}
  COMPATIBILITY AnyNewerVersion
)

install(TARGETS sqlite EXPORT sqlite
  RUNTIME DESTINATION bin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib)


install(FILES
  ${CMAKE_CURRENT_SOURCE_DIR}/sqlite3.h
  ${CMAKE_CURRENT_SOURCE_DIR}/sqlite3ext.h
  DESTINATION include)

install(EXPORT sqlite
  FILE
    sqlite.cmake
  NAMESPACE
    sqlite::
  DESTINATION
    lib/cmake/sqlite
)
install(
  FILES
    cmake/sqlite-config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/sqlite-config-version.cmake
  DESTINATION
    lib/cmake/sqlite
)