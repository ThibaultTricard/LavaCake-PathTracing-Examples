cmake_minimum_required (VERSION 3.0)
project( "LavaCake-RayTracing-Examples" )


set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


add_subdirectory(LavaCake)

###############################################################
# Sample projects                                             #
###############################################################

file( COPY "${CMAKE_CURRENT_LIST_DIR}/Samples/Data" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}" )
# Sample projects generation
macro( list_samples )
		file( GLOB SAMPLES RELATIVE "${CMAKE_CURRENT_LIST_DIR}/Samples/Source Files" "${CMAKE_CURRENT_LIST_DIR}/Samples/Source Files/*" )

		FOREACH( SAMPLE ${SAMPLES} )
			file( GLOB SAMPLE_SOURCE_FILES "Samples/Source Files/${SAMPLE}/*.cpp" )
			source_group( "" FILES ${SAMPLE_SOURCE_FILES} )

			clearShader()
			file( GLOB SHADER_SOURCE_FILES "Samples/Data/Shaders/${SAMPLE}/*")
			FOREACH( shader ${SHADER_SOURCE_FILES} )
				string(REGEX MATCH "[A-Za-z0-9_\\-]*\\.[A-Za-z0-9]*" FILENAME ${shader})
				addShader(${shader} ${CMAKE_CURRENT_BINARY_DIR}/Data/Shaders/${SAMPLE}/${FILENAME}.spv)
			endforeach()
			add_executable( ${SAMPLE} ${SAMPLE_SOURCE_FILES} )
			target_link_libraries( ${SAMPLE} LavaCake )
			AutoSPIRV(${SAMPLE})
			set_property( TARGET ${SAMPLE} PROPERTY FOLDER "Samples" )
			get_property(PATH GLOBAL PROPERTY HELPER_PATH)
			

		ENDFOREACH()
endmacro()



# Sample projects generation
list_samples()


