# SPDX-License-Identifier: BSD-3-Clause
# Copyright Contributors to the OpenColorIO Project.

if(NOT OCIO_GL_ENABLED)
    message(WARNING "GL component missing. GPU disabled for ocioconvert.")
else()
    add_compile_definitions(OCIO_GPU_ENABLED)
    set(OCIO_GL_LIB oglapphelpers)
endif()

set(SOURCES
    main.cpp
)

add_executable(ocioconvert ${SOURCES})

set_target_properties(ocioconvert PROPERTIES 
    COMPILE_FLAGS "${PLATFORM_COMPILE_FLAGS}")

if (USE_MSVC)
	# Temporary until fixed in OpenImageIO: Mute some warnings from OpenImageIO farmhash.h
	# C4267 (level 3)	    'var' : conversion from 'size_t' to 'type', possible loss of data
	# C4244	(level 3 & 4)	'conversion' conversion from 'type1' to 'type2', possible loss of data
    target_compile_options(ocioconvert PRIVATE /wd4267 /wd4244)
endif()

target_link_libraries(ocioconvert
    PRIVATE
        apputils
        ${OCIO_GL_LIB}
        oiiohelpers
        OpenColorIO
        OpenImageIO::OpenImageIO
)

install(TARGETS ocioconvert
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
