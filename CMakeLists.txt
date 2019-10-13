project (ork.fcollada)
include (GenerateExportHeader)
###################################

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED on)

set(CMAKE_INSTALL_RPATH "$ENV{OBT_STAGE}/lib")
set(CMAKE_BUILD_WITH_INSTALL_RPATH ON)

IF(${APPLE})
set(CMAKE_MACOSX_RPATH 1)
ENDIF()

add_compile_options(-Wno-deprecated -Wno-register -fexceptions)
add_compile_options(-Wno-unused-command-line-argument)
add_compile_options(-fPIE -fPIC -fno-common -fno-strict-aliasing -g -Wno-switch-enum)

set( SRCD ${CMAKE_CURRENT_SOURCE_DIR} )

###################################

file(GLOB src_1 ${SRCD}/FCollada/*.cpp)
file(GLOB src_2 ${SRCD}/FCollada/LibXML/*.c)
file(GLOB src_3 ${SRCD}/FCollada/FCDocument/*.cpp)
file(GLOB src_4 ${SRCD}/FCollada/FMath/*.cpp)
file(GLOB src_5 ${SRCD}/FCollada/FUtils/*.cpp)
file(GLOB src_6 ${SRCD}/FCollada/Output/*.cpp)
file(GLOB src_7 ${SRCD}/FCollada/Output/*.cpp)
file(GLOB src_8 ${SRCD}/FColladaPlugins/FArchiveXML/*.cpp)

###################################

set( SRCS ${src_1} ${src_2} ${src_3} ${src_4} ${src_5} )
list( APPEND SRCS ${src_6} ${src_7} ${src_8} )
add_library (ork_fcollada SHARED ${SRCS} )

###################################

IF(${APPLE})
    target_include_directories (ork_fcollada PRIVATE /usr/local/include)
    target_link_libraries(ork_fcollada LINK_PUBLIC m pthread)
    target_link_libraries(ork_fcollada LINK_PUBLIC
        "-framework AppKit"
        "-framework IOKit"
    )
    target_link_libraries(ork_fcollada LINK_PUBLIC objc )
ELSEIF(${UNIX})
    target_link_libraries(ork_fcollada LINK_PUBLIC rt dl pthread)
ENDIF()

target_include_directories (ork_fcollada PRIVATE ${SRCD}/FCollada )
target_include_directories (ork_fcollada PRIVATE ${SRCD}/FCollada/LibXML/include )

###################################

install(TARGETS ork_fcollada LIBRARY DESTINATION $ENV{OBT_STAGE}/lib )
