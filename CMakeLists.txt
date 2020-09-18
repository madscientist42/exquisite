project(exquisite)
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

find_package(Eina REQUIRED)
find_package(Eet REQUIRED)
find_package(Evas REQUIRED)
find_package(Ecore REQUIRED)
find_package(Ecore COMPONENTS Evas)
find_package(Ecore COMPONENTS con)
find_package(Edje REQUIRED)
find_package(Efreet REQUIRED)

# Define an option to build the theme in the CMake build.  This is
# to allow someone with a local build on a target machine to test/play
# with the theme in the theme/ directory using the demo script.
option(BUILD_THEME "Build the theme config for the demo" OFF)

# Set the destination for local builds so we can actually run the demo
# by hand...
set(CMAKE_BINARY_DIR "bin")

include_directories (
    ${PROJECT_SOURCE_DIR}/include
    ${EINA_INCLUDE_DIRS}
    ${EET_INCLUDE_DIRS}
    ${EVAS_INCLUDE_DIRS}
    ${ECORE_INCLUDE_DIRS}
    ${ECORE_EVAS_INCLUDE_DIRS}
    ${ECORE_CON_INCLUDE_DIRS}
    ${EDJE_INCLUDE_DIRS}
    ${EFREET_INCLUDE_DIRS}
    )

add_library(Exquisite SHARED src/exquisite.c)
target_link_libraries(Exquisite
    ${EINA_LIBRARIES}
    ${EET_LIBRARIES}
    ${EVAS_LIBRARIES}
    ${ECORE_LIBRARIES}
    ${ECORE_EVAS_LIBRARIES}
    ${ECORE_CON_LIBRARIES}
    libedje.so
    ${EFREET_LIBRARIES})

add_executable(exquisite src/ipc.c src/theme.c src/main.c)
target_link_libraries(exquisite Exquisite
    ${EINA_LIBRARIES}
    ${EET_LIBRARIES}
    ${EVAS_LIBRARIES}
    ${ECORE_LIBRARIES}
    ${ECORE_EVAS_LIBRARIES}
    ${ECORE_CON_LIBRARIES}
    libedje.so
    ${EFREET_LIBRARIES})

add_executable(exquisite_write src/write.c)
target_link_libraries(exquisite_write Exquisite
    ${ECORE_LIBRARIES}
    ${ECORE_CON_LIBRARIES})

# Add a builder for the theme so that someone can run the demo in the source
# tree as needed...
if (BUILD_THEME)
    add_custom_command(OUTPUT ${CMAKE_BINARY_DIR}/default.edj
        COMMAND edje_cc -fd ${CMAKE_SOURCE_DIR}/theme/fonts -id ${CMAKE_SOURCE_DIR}/theme/images ${CMAKE_SOURCE_DIR}/theme/default.edc ${CMAKE_BINARY_DIR}/default.edj
        DEPENDS ${CMAKE_SOURCE_DIR}/theme/default.edc)
endif()

install(TARGETS exquisite exquisite_write DESTINATION bin)
install(TARGETS Exquisite DESTINATION lib)

