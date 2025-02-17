
set( SOURCE_FILES
      rsslConsumer.c           rsslDictionaryHandler.c        rsslDirectoryHandler.c
      rsslMarketPriceHandler.c rsslMarketByOrderHandler.c     rsslMarketByPriceHandler.c
      rsslSymbolListHandler.c  rsslYieldCurveHandler.c        rsslPostHandler.c
      ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslDirectoryEncodeDecode.c	
      ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslLoginConsumer.c
      ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslLoginEncodeDecode.c     
      ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslSendMessage.c
      ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslMarketPriceItems.c
	  ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslJsonSession.c

	  rsslConsumer.h           rsslDictionaryHandler.h        rsslDirectoryHandler.h
      rsslMarketPriceHandler.h rsslMarketByOrderHandler.h     rsslMarketByPriceHandler.h
      rsslSymbolListHandler.h  rsslYieldCurveHandler.h        rsslPostHandler.h
	  ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslDirectoryEncodeDecode.h	
      ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslLoginConsumer.h
      ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslLoginEncodeDecode.h     
      ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslSendMessage.h
      ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslMarketPriceItems.h
	  ${EtaExamples_SOURCE_DIR}/Examples/Common/rsslJsonSession.h
  )

add_executable( Consumer_shared ${SOURCE_FILES} )
target_include_directories(Consumer_shared
							PUBLIC
								$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
								$<BUILD_INTERFACE:${EtaExamples_SOURCE_DIR}/Examples/Common>
								$<BUILD_INTERFACE:${Eta_SOURCE_DIR}/Include/Converter>
                )

add_executable( Consumer ${SOURCE_FILES} )
target_include_directories(Consumer
							PUBLIC
								$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
								$<BUILD_INTERFACE:${EtaExamples_SOURCE_DIR}/Examples/Common>
								$<BUILD_INTERFACE:${Eta_SOURCE_DIR}/Include/Converter>
							)

set_target_properties( Consumer Consumer_shared
                        PROPERTIES 
                            OUTPUT_NAME Consumer )

target_link_libraries( Consumer librssl ${SYSTEM_LIBRARIES} )
target_link_libraries( Consumer_shared librssl_shared ${SYSTEM_LIBRARIES} )

if ( CMAKE_HOST_WIN32 )
    set_target_properties( Consumer 
                            PROPERTIES 
                                PROJECT_LABEL "Consumer" 
                        )
	target_compile_options( Consumer 
								PRIVATE 
									${RCDEV_DEBUG_TYPE_FLAGS_NONSTATIC}
									${RCDEV_TYPE_CHECK_FLAG}
									$<$<CONFIG:Release_MD>:${RCDEV_FLAGS_NONSTATIC_RELEASE}>
						)

    set_target_properties( Consumer_shared 
                            PROPERTIES 
                                PROJECT_LABEL "Consumer_shared" 
                                RUNTIME_OUTPUT_DIRECTORY_RELEASE_MD
                                    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE_MD}/Shared
                                RUNTIME_OUTPUT_DIRECTORY_DEBUG_MDD
                                    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG_MDD}/Shared
								LIBRARY_OUTPUT_DIRECTORY_RELEASE_MD
                                    ${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE_MD}/Shared
                                LIBRARY_OUTPUT_DIRECTORY_DEBUG_MDD
                                    ${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG_MDD}/Shared
								PDB_OUTPUT_DIRECTORY_RELEASE_MD 
									${CMAKE_PDB_OUTPUT_DIRECTORY_RELEASE_MD}/Shared 
								PDB_OUTPUT_DIRECTORY_DEBUG_MDD 
									${CMAKE_PDB_OUTPUT_DIRECTORY_DEBUG_MDD}/Shared
								ARCHIVE_OUTPUT_DIRECTORY_RELEASE_MD
                                    ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE_MD}/Shared
                                ARCHIVE_OUTPUT_DIRECTORY_DEBUG_MDD
                                    ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG_MDD}/Shared
                        )
	target_compile_options( Consumer_shared 
								PRIVATE 
									${RCDEV_DEBUG_TYPE_FLAGS_NONSTATIC}
									${RCDEV_TYPE_CHECK_FLAG}
									$<$<CONFIG:Release_MD>:${RCDEV_FLAGS_NONSTATIC_RELEASE}>
						)


else()
	set_target_properties( Consumer_shared 
                            PROPERTIES 
                                RUNTIME_OUTPUT_DIRECTORY 
                                    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/Shared 
                            )
endif()
