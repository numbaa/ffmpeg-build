project(ffmpeg)

add_library(${PROJECT_NAME}::avcodec SHARED IMPORTED GLOBAL)
add_library(${PROJECT_NAME}::avutil SHARED IMPORTED GLOBAL)

set_target_properties(${PROJECT_NAME}::avcodec PROPERTIES INTERFACE_LINK_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/bin")
set_target_properties(${PROJECT_NAME}::avcodec PROPERTIES IMPORTED_IMPLIB "${CMAKE_CURRENT_SOURCE_DIR}/bin/avcodec.lib")
set_target_properties(${PROJECT_NAME}::avcodec PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/bin/avcodec-59.dll")

set_target_properties(${PROJECT_NAME}::avutil PROPERTIES INTERFACE_LINK_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/bin")
set_target_properties(${PROJECT_NAME}::avutil PROPERTIES IMPORTED_IMPLIB "${CMAKE_CURRENT_SOURCE_DIR}/bin/avutil.lib")
set_target_properties(${PROJECT_NAME}::avutil PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/bin/avutil-57.dll")

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/include")
set_property(TARGET ${PROJECT_NAME} PROPERTY INTERFACE_LINK_LIBRARIES
	${PROJECT_NAME}::avcodec
	${PROJECT_NAME}::avutil
)

install(
	FILES
		${CMAKE_CURRENT_SOURCE_DIR}/bin/avcodec-59.dll
		${CMAKE_CURRENT_SOURCE_DIR}/bin/avutil-57.dll
	DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
)