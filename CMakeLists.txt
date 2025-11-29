cmake_minimum_required(VERSION 3.12)

project(lwgeom C)

set(CMAKE_INSTALL_PREFIX /usr/local)

file(GLOB LWGEOM_SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/liblwgeom/*.c
    ${CMAKE_CURRENT_SOURCE_DIR}/liblwgeom/ryu/*.c
)

include_directories(./)
include_directories(./liblwgeom)

add_library(lwgeom SHARED ${LWGEOM_SRC})

set_target_properties(lwgeom PROPERTIES
    LINKER_LANGUAGE C
)

target_compile_options(lwgeom PRIVATE -std=gnu11)

target_link_libraries(lwgeom PUBLIC geos_c)
target_link_libraries(lwgeom PUBLIC proj)
target_link_libraries(lwgeom PUBLIC SFCGAL)

install(TARGETS lwgeom
    LIBRARY DESTINATION lib
)

install(FILES postgis_config.h postgis_revision.h DESTINATION include)

install(
    DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/liblwgeom/
    DESTINATION include/lwgeom
    FILES_MATCHING
    PATTERN "*.h"
)
