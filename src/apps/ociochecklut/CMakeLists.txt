# SPDX-License-Identifier: BSD-3-Clause
# Copyright Contributors to the OpenColorIO Project.

if(NOT OCIO_GL_ENABLED)
    message(WARNING "GL component missing. GPU disabled for ociochecklut.")
else()
    add_compile_definitions(OCIO_GPU_ENABLED)
    set(OCIO_GL_LIB oglapphelpers)
endif()

set(SOURCES
    main.cpp
)

add_executable(ociochecklut ${SOURCES})

if(MSVC)
    set(PLATFORM_COMPILE_OPTIONS "${PLATFORM_COMPILE_OPTIONS};/wd4996")
endif()

set_target_properties(ociochecklut PROPERTIES 
    COMPILE_OPTIONS "${PLATFORM_COMPILE_OPTIONS}"
    LINK_OPTIONS "${PLATFORM_LINK_OPTIONS}"
)

target_link_libraries(ociochecklut 
    PRIVATE 
        ${OCIO_GL_LIB}
        apputils
        OpenColorIO
)

install(TARGETS ociochecklut
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
