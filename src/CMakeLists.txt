########################################################################
IF(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
    MESSAGE(FATAL_ERROR "Prevented in-tree built. This is bad practice.")
ENDIF(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})

########################################################################
# Project setup
########################################################################



include_directories(${CMAKE_CURRENT_SOURCE_DIR})

aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR} UIO_SRCS)



ADD_LIBRARY(uio SHARED ${UIO_SRCS})
TARGET_LINK_LIBRARIES(uio ${LIBS})
ADD_LIBRARY(uio_static STATIC ${UIO_SRCS})
set_target_properties(uio_static PROPERTIES OUTPUT_NAME "uio")
set_target_properties(uio PROPERTIES CLEAN_DIRECT_OUTPUT 1)
set_target_properties(uio_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)
set_target_properties(uio PROPERTIES 
    ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/bin")
set_target_properties(uio_static PROPERTIES 
    ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}/bin")

#IF(UNIX)
#    SET_PROPERTY(TARGET uio PROPERTY VERSION ${ZYNQ_HARDWARE_VERSION})
#    SET_PROPERTY(TARGET uio PROPERTY SOVERSION ${ZYNQ_HARDWARE_VERSION_MAJOR})
#ENDIF(UNIX)
INSTALL(TARGETS uio DESTINATION lib)
INSTALL(TARGETS uio_static DESTINATION lib)
INSTALL(DIRECTORY . DESTINATION include FILES_MATCHING PATTERN "*.h")
#INSTALL(FILES ${CMAKE_CURRENT_SOURCE_DIR}/uio.h DESTINATION include)
#INSTALL(FILES ${CMAKE_CURRENT_SOURCE_DIR}/stream_func.h DESTINATION include)
#INSTALL(FILES ${CMAKE_CURRENT_SOURCE_DIR}/record_media.h DESTINATION include)
#INSTALL(FILES ${CMAKE_CURRENT_SOURCE_DIR}/discovery/broadcast_client.h DESTINATION include)