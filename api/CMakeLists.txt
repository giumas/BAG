# 
# Hdf5 Settings
#
IF(MSVC)
    # Definition required by HDF5 lib
    ADD_DEFINITIONS(-D H5_BUILT_AS_DYNAMIC_LIB)
    # The location of the Hdf5 header files.
    SET(HDF5_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/extlibs/hdf5/installed/include" CACHE PATH "Location of hdf5 header files.")
    # The name of the Hdf5 release library.
    SET(HDF5_LIB "optimized;${PROJECT_SOURCE_DIR}/extlibs/hdf5/installed/lib/hdf5.lib;debug;${PROJECT_SOURCE_DIR}/extlibs/hdf5/installed/lib/hdf5_D.lib" CACHE STRING "The name of the hdf5 release library.")
ELSEIF(UNIX)
    # The location of the Hdf5 header files.
    # SET(HDF5_INCLUDE_DIR "/usr/include" CACHE PATH "Location of hdf5 header files.")
    # The name of the Hdf5 release library.
    # SET(HDF5_LIB "hdf5" CACHE STRING "The name of the hdf5 release library.")
    find_package(HDF5 REQUIRED)
    set(HDF5_INCLUDE_DIR ${HDF5_INCLUDE_DIRS} CACHE PATH "Location of hdf5 header files.")
    set(HDF5_LIB ${HDF5_LIBRARIES})
ELSEIF(MINGW)
    find_package(HDF5 REQUIRED)
    if(HDF5_C_LIBRARY)
        set(HDF5_LIB ${HDF5_C_LIBRARY})
    else(HDF5_C_LIBRARY)
        # The name of the Hdf5 release library.
        SET(HDF5_LIB "${PROJECT_SOURCE_DIR}/extlibs/hdf5/installed/lib/hdf5" CACHE STRING "The name of the hdf5 release library.")
    endif(HDF5_C_LIBRARY)

    # The location of the Hdf5 header files.
    SET(HDF5_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/extlibs/hdf5/installed/include" CACHE PATH "Location of hdf5 header files.")
ENDIF(MSVC)

# 
# beecrypt Settings
#
IF(MSVC)
    # The location of the beecrypt header files.
    SET(BEECRYPT_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/extlibs/" CACHE PATH "Location of beecrypt header files.")
    # The name of the beecrypt release library.
    SET(BEECRYPT_LIB "optimized;${PROJECT_SOURCE_DIR}/extlibs/beecrypt/beecrypt.lib;debug;${PROJECT_SOURCE_DIR}/extlibs/beecrypt/beecryptd.lib" CACHE STRING "The name of the beecrypt release library.")
ELSEIF(UNIX)
    # The location of the beecrypt header files.
    #SET(BEECRYPT_INCLUDE_DIR "/usr/include" CACHE PATH "Location of beecrypt header files.")
    FIND_PATH(BEECRYPT_INCLUDE_DIR beecrypt/aes.h)
    # The name of the beecrypt release library.
    #SET(BEECRYPT_LIB "beecrypt" CACHE STRING "The name of the beecrypt release library.")
    FIND_LIBRARY(BEECRYPT_LIB beecrypt DOC "The name of the beecrypt release library.")
ELSEIF(MINGW)
    # The location of the beecrypt header files.
    SET(BEECRYPT_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/extlibs/beecrypt/installed/include/" CACHE PATH "Location of beecrypt header files.")
    # The name of the beecrypt release library.
    SET(BEECRYPT_LIB "${PROJECT_SOURCE_DIR}/extlibs/beecrypt/installed/lib/beecrypt" CACHE STRING "The name of the beecrypt release library.")
ENDIF(MSVC)

# 
# libxml Settings
#
IF(MSVC)
    # The location of the libxml2 header files.
    SET(LIBXML_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/extlibs/libxml2/include" CACHE PATH "Location of libxml2 header files.")
    # The name of the libxml2 release library.
    SET(LIBXML_LIB "optimized;${PROJECT_SOURCE_DIR}/extlibs/libxml2/lib/release/libxml2.lib;debug;${PROJECT_SOURCE_DIR}/extlibs/libxml2/lib/debug/libxml2.lib" CACHE STRING "The name of the libxml2 library.")
ELSEIF(UNIX)

    # The location of the libxml2 header files.
    FIND_PACKAGE(LibXml2)
    IF(LIBXML2_INCLUDE_DIR)
        SET(LIBXML_INCLUDE_DIR ${LIBXML2_INCLUDE_DIR} CACHE PATH "Location of libxml2 header files.")
        SET(LIBXML_LIB ${LIBXML2_LIBRARIES} CACHE STRING "The name of the libxml2 release library.")
    ELSE(LIBXML2_INCLUDE_DIR)
        SET(LIBXML_INCLUDE_DIR "/usr/include" CACHE PATH "Location of libxml2 header files.")
        # The name of the libxml2 release library.
        SET(LIBXML_LIB "libxml2" CACHE STRING "The name of the libxml2 release library.")
    ENDIF(LIBXML2_INCLUDE_DIR)

ELSEIF(MINGW)
    # The location of the libxml2 header files.
    FIND_PACKAGE(LibXml2)
    IF(LIBXML2_INCLUDE_DIR)
        SET(LIBXML_INCLUDE_DIR ${LIBXML2_INCLUDE_DIR} CACHE PATH "Location of libxml2 header files.")
        SET(LIBXML_LIB ${LIBXML2_LIBRARIES} CACHE STRING "The name of the libxml2 release library.")
    ELSE(LIBXML2_INCLUDE_DIR)
        SET(LIBXML_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/extlibs/libxml2/installed/include/" CACHE PATH "Location of libxml2 header files.")
        # The name of the libxml2 release library.
        SET(LIBXML_LIB "${PROJECT_SOURCE_DIR}/extlibs/libxml2/installed/lib/libxml2" CACHE STRING "The name of the libxml2 release library.")
    ENDIF(LIBXML2_INCLUDE_DIR)
ENDIF(MSVC)

include_directories (${HDF5_INCLUDE_DIR} ${BEECRYPT_INCLUDE_DIR} ${LIBXML_INCLUDE_DIR}) 

# The debug build will have a 'd' postfix
SET(CMAKE_DEBUG_POSTFIX  "d")

if(WIN32)
	#Preset install dir
	IF(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
		SET(CMAKE_INSTALL_PREFIX "${PROJECT_SOURCE_DIR}/installed" CACHE PATH "CMAKE_INSTALL_PREFIX" FORCE)
	ENDIF(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
endif()

#Use the version 1 interface for HDF5
add_definitions(-D H5_USE_16_API)

IF(MSVC)

    #ignore some warnings generated by windows.
    add_definitions(/D_CRT_SECURE_NO_WARNINGS /D _CRT_NONSTDC_NO_DEPRECATE)
    
	#if we are building a shared library, then we need to define these symbols.
	IF(BUILD_SHARED_LIBS)
		add_definitions(/D BAG_DLL /D BAG_EXPORTS)
	ENDIF(BUILD_SHARED_LIBS)
	
	#if we are building against a shared HDF5, then we need to define this symbol.
	IF(BUILD_WITH_SHARED_HDF5)
		add_definitions(/D _HDF5USEDLL_)
	ENDIF(BUILD_WITH_SHARED_HDF5)
	
ENDIF(MSVC)

#List all of the bag api source files.
set (BAG_SOURCE_FILES 
 bag_attr.c
 bag_crypto.c
 bag_hdf.c
 bag_metadata.cpp
 bag_metadata_export.cpp
 bag_metadata_import.cpp
 bag_legacy.c
 bag_opt_group.c
 bag_opt_surfaces.c
 bag_reference_system.cpp
 bag_surface_correct.c
 bag_surfaces.c
 bag_tracking_list.c
 crc32.c
 onscrypto.c)
source_group("Source Files" FILES ${BAG_SOURCE_FILES})
 
set (BAG_HEADER_FILES 
 bag.h
 bag_config.h
 bag_errors.h
 bag_metadata.h
 bag_legacy.h
 bag_opt_surfaces.h
 bag_private.h
 crc32.h
 dt_cc.h
 onscrypto.h
 stdtypes.h)
source_group("Header Files" FILES ${BAG_HEADER_FILES})
 
add_library (bag ${LIB_TYPE} ${BAG_SOURCE_FILES} ${BAG_HEADER_FILES} )

target_link_libraries(bag ${HDF5_LIB})
target_link_libraries(bag ${BEECRYPT_LIB})
target_link_libraries(bag ${LIBXML_LIB})

IF(MSVC)

    #Apparently some versions of Visual Studio require us to link to ws2_32.lib
    target_link_libraries(bag "ws2_32.lib")
    
ENDIF(MSVC)

install(FILES ${BAG_HEADER_FILES} DESTINATION include)

install(TARGETS bag
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib/static)
        
if(MSVC)

    #Copy the DLL dependencies
    install(FILES ${PROJECT_SOURCE_DIR}/extlibs/hdf5/installed/bin/hdf5.dll DESTINATION bin)
    install(DIRECTORY ${PROJECT_SOURCE_DIR}/extlibs/zlib/installed/bin/ DESTINATION bin)
    install(FILES ${PROJECT_SOURCE_DIR}/extlibs/beecrypt/beecrypt.dll DESTINATION bin)
    install(FILES ${PROJECT_SOURCE_DIR}/extlibs/libxml2/bin/release/libxml2.dll DESTINATION bin)
    
endif()

install(DIRECTORY ${PROJECT_SOURCE_DIR}/configdata/ DESTINATION configdata)
