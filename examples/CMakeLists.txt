

set( 
  REQUIRED_CISST_LIBRARIES 
  cisstCommon
  cisstVector
  cisstOSAbstraction
  cisstMultiTask )

find_package( cisst REQUIRED ${REQUIRED_CISST_LIBRARIES} )
find_package( sawCANBus REQUIRED HINTS "${cisst_saw_BINARY_DIR}/saw/components")

if( cisst_FOUND_AS_REQUIRED )

  # load cisst configuration
  include( ${CISST_USE_FILE} )

  if( sawCANBus_FOUND )
    add_executable( osaRTSocketCANRecvExample     osaRTSocketCANRecvExample.cpp )
    add_executable( osaRTSocketCANSendExample     osaRTSocketCANSendExample.cpp )
    add_executable( mtsRTSocketCANSendRecvExample mtsRTSocketCANSendRecvExample.cpp )

    target_link_libraries( osaRTSocketCANRecvExample sawCANBus )
    target_link_libraries( osaRTSocketCANSendExample sawCANBus )
    target_link_libraries( mtsRTSocketCANSendRecvExample sawCANBus )

    cisst_target_link_libraries( osaRTSocketCANRecvExample ${REQUIRED_CISST_LIBRARIES} )
    cisst_target_link_libraries( osaRTSocketCANSendExample ${REQUIRED_CISST_LIBRARIES} )
    cisst_target_link_libraries( mtsRTSocketCANSendRecvExample ${REQUIRED_CISST_LIBRARIES} )

  endif( sawCANBus_FOUND )
  
  


endif( cisst_FOUND_AS_REQUIRED )