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

install(TARGETS exquisite exquisite_write DESTINATION bin)
install(TARGETS Exquisite DESTINATION lib)

