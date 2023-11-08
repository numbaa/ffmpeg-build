project(ffmpeg)

add_library(${PROJECT_NAME}::avcodec SHARED IMPORTED GLOBAL)
add_library(${PROJECT_NAME}::avutil SHARED IMPORTED GLOBAL)


if (LT_WINDOWS)
	set(AVCODEC_LIB ${CMAKE_CURRENT_SOURCE_DIR}/bin/avcodec-59.dll)
	set(AVUTIL_LIB ${CMAKE_CURRENT_SOURCE_DIR}/bin/avutil-57.dll)
	set_target_properties(${PROJECT_NAME}::avcodec PROPERTIES INTERFACE_LINK_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/bin")
	set_target_properties(${PROJECT_NAME}::avcodec PROPERTIES IMPORTED_IMPLIB "${CMAKE_CURRENT_SOURCE_DIR}/bin/avcodec.lib")
	set_target_properties(${PROJECT_NAME}::avcodec PROPERTIES IMPORTED_LOCATION ${AVCODEC_LIB})
	set_target_properties(${PROJECT_NAME}::avutil PROPERTIES INTERFACE_LINK_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/bin")
	set_target_properties(${PROJECT_NAME}::avutil PROPERTIES IMPORTED_IMPLIB "${CMAKE_CURRENT_SOURCE_DIR}/bin/avutil.lib")
	set_target_properties(${PROJECT_NAME}::avutil PROPERTIES IMPORTED_LOCATION  ${AVUTIL_LIB})

elseif(LT_LINUX)
	set(AVCODEC_LIB ${CMAKE_CURRENT_SOURCE_DIR}/lib/libavcodec.so.59.37.100)
	set(AVUTIL_LIB ${CMAKE_CURRENT_SOURCE_DIR}/lib/libavutil.so.57.28.100)
	set_target_properties(${PROJECT_NAME}::avcodec PROPERTIES INTERFACE_LINK_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/lib")
	set_target_properties(${PROJECT_NAME}::avcodec PROPERTIES IMPORTED_LOCATION ${AVCODEC_LIB})
	set_target_properties(${PROJECT_NAME}::avutil PROPERTIES INTERFACE_LINK_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/lib")
	set_target_properties(${PROJECT_NAME}::avutil PROPERTIES IMPORTED_LOCATION ${AVUTIL_LIB})

else()

endif()

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/include")
set_property(TARGET ${PROJECT_NAME} PROPERTY INTERFACE_LINK_LIBRARIES
	${PROJECT_NAME}::avcodec
	${PROJECT_NAME}::avutil
)

install(
	FILES
		${AVCODEC_LIB}
		${AVUTIL_LIB}
	DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
)