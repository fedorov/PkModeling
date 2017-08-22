
#
# SlicerExecutionModel
#
find_package(SlicerExecutionModel REQUIRED)
include(${SlicerExecutionModel_USE_FILE})

#-----------------------------------------------------------------------------
set(MODULE_NAME PkModeling)

#-----------------------------------------------------------------------------
set(MODULE_INCLUDE_DIRECTORIES
  ${${MODULE_NAME}_SOURCE_DIR}/PkSolver
  )

#-----------------------------------------------------------------------------
set(MODULE_SRCS
  ${MODULE_NAME}.cxx
  itkSignalIntensityToS0ImageFilter.h
  itkSignalIntensityToS0ImageFilter.hxx
  itkSignalIntensityToConcentrationImageFilter.h
  itkSignalIntensityToConcentrationImageFilter.h
  itkConcentrationToQuantitativeImageFilter.h
  itkConcentrationToQuantitativeImageFilter.hxx
  )

#-----------------------------------------------------------------------------
set(MODULE_TARGET_LIBRARIES
  ${ITK_LIBRARIES} PkSolver
  )

#
# ITK
#
#set(ITK_NO_IO_FACTORY_REGISTER_MANAGER 1) # See Libs/ITKFactoryRegistration/CMakeLists.txt
#include(${ITK_USE_FILE})

#-----------------------------------------------------------------------------
SEMMacroBuildCLI(
  NAME ${MODULE_NAME}
  INCLUDE_DIRECTORIES ${MODULE_INCLUDE_DIRECTORIES}
  ADDITIONAL_SRCS ${MODULE_SRCS}
  TARGET_LIBRARIES ${MODULE_TARGET_LIBRARIES}
  )


set(lib_name PkModeling)

#add_library(${lib_name} STATIC
#  ${MODULE_SRCS}
#  )

if(PkModeling_LIBRARY_PROPERTIES)
  set_target_properties(${lib_name} PROPERTIES ${PkModeling_LIBRARY_PROPERTIES})
endif()

set_property(GLOBAL APPEND PROPERTY ${CMAKE_PROJECT_NAME}_TARGETS ${lib_name})

set(${lib_name}_INCLUDE_DIRS
  ${DCMTK_INCLUDE_DIRS}
  ${ITK_INCLUDE_DIRS}
  )
if(ITK_INSTALL_PREFIX)
  list(APPEND ${lib_name}_INCLUDE_DIRS
    ${ITK_INSTALL_PREFIX}/include/vxl/core
    ${ITK_INSTALL_PREFIX}/include/vxl/vcl
    )
endif()
target_include_directories(${lib_name} PUBLIC ${${lib_name}_INCLUDE_DIRS})

target_link_libraries(${lib_name} PUBLIC
  ${DCMTK_LIBRARIES}
  ${ITK_LIBRARIES}
  )











#-----------------------------------------------------------------------------
if(BUILD_TESTING)
  add_subdirectory(Testing)
endif()

