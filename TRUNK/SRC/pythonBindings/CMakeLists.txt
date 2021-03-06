PROJECT(EMBR_PYTHON_WRAPPER)

INCLUDE_DIRECTORIES(  ${PYTHON_INCLUDE_PATH}
                      ${DFKI_REALIZER_SOURCE_DIR}
                      ${DFKI_REALIZER_PYHON_WRAPPER_SOURCE_DIR}
                      ${SAMSARA_CORE_SOURCE_DIR}
                      ${SAMSARA_MATH_SOURCE_DIR}
                      ${SAMSARA_COMMON_SOURCE_DIR}
                      ${SAMSARA_NEWMAT_SOURCE_DIR}
                      ${LOG4CPLUS_INCLUDE_DIR}
                   )

SOURCE_GROUP(Headers REGULAR_EXPRESSION "[.]h$")

LINK_DIRECTORIES(       ${LIBRARY_OUTPUT_PATH}
                )

ADD_LIBRARY( SMRPy SHARED
             Bindings.cpp
           )

TARGET_LINK_LIBRARIES( SMRPy
            realizer
            io
            newmat
            common
            core
            math
            ${LOG4CPLUS_LIBRARIES}
            ${Boost_PYTHON_LIBRARY}
	        ${Boost_THREAD_LIBRARY}
	        ${Boost_DATE_TIME_LIBRARY}
            ${Boost_SYSTEM_LIBRARY}
	        ${PYTHON_LIBRARY}
	        ${LIBXML2_LIBRARIES}
            )

IF(WIN32)
  TARGET_LINK_LIBRARIES(SMRPy actuators motionsegments Ws2_32.lib )
ENDIF(WIN32)

IF(UNIX)
  TARGET_LINK_LIBRARIES(SMRPy actuators motionsegments ${ZLIB_LIBRARIES} ${SAMARA_ICONV_LIBRARIES})
ENDIF(UNIX)

IF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
 IF(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  SET(EMBRPYTHONRUNTIME "${LIBRARY_OUTPUT_PATH}/Debug/SMRPy.dll")
  INSTALL(CODE "MESSAGE(\"Copying library file ${EMBRPYTHONRUNTIME}\")")
  INSTALL(FILES ${EMBRPYTHONRUNTIME} DESTINATION ${EXECUTABLE_OUTPUT_PATH}/Debug RENAME SMRPy.pyd)
  INSTALL(FILES ${EMBRPYTHONRUNTIME} DESTINATION ${GAME_PATH} RENAME SMRPy.pyd) 
 ELSE(${CMAKE_BUILD_TYPE} MATCHES "Debug")
  SET(EMBRPYTHONRUNTIME "${LIBRARY_OUTPUT_PATH}/Release/SMRPy.dll")
  INSTALL(CODE "MESSAGE(\"Copying library file ${EMBRPYTHONRUNTIME}\")")
  INSTALL(FILES ${EMBRPYTHONRUNTIME} DESTINATION ${GAME_PATH} RENAME SMRPy.pyd)
 ENDIF(${CMAKE_BUILD_TYPE} MATCHES "Debug")
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")

IF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  IF(${CMAKE_BUILD_TYPE} MATCHES "Debug")
    SET(EMBRPYTHONRUNTIME "${LIBRARY_OUTPUT_PATH}/Debug/libSMRPy.dylib")
    INSTALL(CODE "MESSAGE(\"Copying library file ${EMBRPYTHONRUNTIME}\")")
    #INSTALL(FILES ${EMBRPYTHONRUNTIME} DESTINATION ${EXECUTABLE_OUTPUT_PATH}/Debug RENAME SMRPy.so)
    INSTALL(FILES ${EMBRPYTHONRUNTIME} DESTINATION ${GAME_PATH} RENAME SMRPy.so) 
  ELSE(${CMAKE_BUILD_TYPE} MATCHES "Debug")
    SET(EMBRPYTHONRUNTIME "${LIBRARY_OUTPUT_PATH}/Release/libSMRPy.dylib")
    INSTALL(CODE "MESSAGE(\"Copying library file ${EMBRPYTHONRUNTIME}\")")
    #INSTALL(FILES ${EMBRPYTHONRUNTIME} DESTINATION ${EXECUTABLE_OUTPUT_PATH}/Release RENAME SMRPy.so)
    INSTALL(FILES ${EMBRPYTHONRUNTIME} DESTINATION ${GAME_PATH} RENAME SMRPy.so) 
  ENDIF(${CMAKE_BUILD_TYPE} MATCHES "Debug")
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
