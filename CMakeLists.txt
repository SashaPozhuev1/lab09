cmake_minimum_required(VERSION 3.0) #Верисия cmake

set(CMAKE_CXX_STANDARD 11) #подключаем стандарт c++
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF) #выключаем пользовательские опции

project(print) #название проекта

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp) #добавляем статическую библиотеку

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
) #указывем каталоги для компиляции, PUBLIC, указываем требоваиния использования на основе местоположения.

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp") #добавляем исполняемый файл
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES}) #команды для каждого значения в списке (далее).
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE) #получаем компонент из списка
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE}) #добавляем исполняемый файл из источника.
    target_link_libraries(${EXAMPLE_NAME} print) #добавление библиотек для цели.
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )#правила запуска во время установки
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
) #правила запуска во время установки

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
