
set( 
  REQUIRED_CISST_LIBRARIES 
  cisstCommon
  cisstVector
  cisstOSAbstraction
  cisstMultiTask )

find_package( cisst REQUIRED ${REQUIRED_CISST_LIBRARIES} )

if( cisst_FOUND_AS_REQUIRED )

  # load cisst configuration
  include( ${CISST_USE_FILE} )

  set( 
    HEADER_FILES 
    ../osaCANBus.h 
    ../osaCANBusFrame.h
    ../mtsCANBus.h
    ../mtsCANBusFrame.h )
  
  set( 
    SOURCE_FILES 
    osaCANBus.cpp
    osaCANBusFrame.cpp
    mtsCANBus.cpp
    mtsCANBusFrame.cpp )
  
  # Check for RT Socket CAN
  find_package( RTSocketCAN )
  if( RTSocketCANFound )
    
    set( 
      HEADER_FILES 
      ${HEADER_FILES} 
      ../osaRTSocketCAN.h
      ../mtsRTSocketCAN.h )

    set( 
      SOURCE_FILES 
      ${SOURCE_FILES} 
      osaRTSocketCAN.cpp )
    
    add_definitions( "${RTDM_DEFINITIONS}" )
    include_directories( ${RTDM_INCLUDE_DIR} )
  
  endif( RTSocketCANFound )

  # Check for BitCtrl
  find_package( BitCtrl )
  if( BitCtrlFound )

    set( 
      HEADER_FILES 
      ${HEADER_FILES} 
      ../CiA301.h
      ../CiA402.h
      ../osaCANopen.h   
      ../osaBitCtrl.h )

    set( 
      SOURCE_FILES 
      ${SOURCE_FILES} 
      ../osaCANopen.cpp 
      ../osaBitCtrl.cpp )
    
    include_directories( ${BITCTRL_INCLUDE_DIR} )
    
  endif( BitCtrlFound )

  add_library( sawCANBus ${HEADER_FILES} ${SOURCE_FILES} )
  cisst_target_link_libraries( sawCANBus ${REQUIRED_CISST_LIBRARIES} )

  # create/configure file for find_package (sawCANBus)
  set(sawCANBus_INCLUDE_DIR "${sawComponents_SOURCE_DIR}" "${sawComponents_BINARY_DIR}")
  set( sawCANBus_LIBRARY_DIR "${LIBRARY_OUTPUT_PATH}" )
  set( sawCANBus_LIBRARIES sawCANBus )

  configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/sawCANBusConfig.cmake.in"
    "${sawComponents_BINARY_DIR}/sawCANBusConfig.cmake"
    @ONLY@ )

endif( cisst_FOUND_AS_REQUIRED )
