cmake_minimum_required(VERSION 3.15)
set(PROJECT_NAME "tray_manager")
project(${PROJECT_NAME} LANGUAGES CXX)

set(PLUGIN_NAME "tray_manager_plugin")

add_library(${PLUGIN_NAME} SHARED
  "tray_manager_plugin.cc"
)

apply_standard_settings(${PLUGIN_NAME})

set_target_properties(${PLUGIN_NAME} PROPERTIES
  CXX_VISIBILITY_PRESET hidden
)

target_compile_definitions(${PLUGIN_NAME} PRIVATE
  FLUTTER_PLUGIN_IMPL
  _SILENCE_CXX17_CODECVT_HEADER_DEPRECATION_WARNING
)

target_include_directories(${PLUGIN_NAME} INTERFACE
  "${CMAKE_CURRENT_SOURCE_DIR}/include"
)

find_package(PkgConfig REQUIRED)

pkg_check_modules(AYATANA REQUIRED ayatana-appindicator3-0.1)

target_compile_definitions(${PLUGIN_NAME} PRIVATE
  HAVE_AYATANA
)

target_include_directories(${PLUGIN_NAME} PRIVATE
  ${AYATANA_INCLUDE_DIRS}
)

target_link_directories(${PLUGIN_NAME} PRIVATE
  ${AYATANA_LIBRARY_DIRS}
)

target_link_libraries(${PLUGIN_NAME} PRIVATE
  flutter
  flutter_wrapper_plugin
  ${AYATANA_LIBRARIES}
)

set(tray_manager_bundled_libraries
  ""
  PARENT_SCOPE
)
