# ==============================================================================
# Generic CMake File for QTBased project
# ==============================================================================

# The name of my project.
SET(ProjectName ui_openMVG_control_points_registration)
PROJECT(${ProjectName})

# CMake version control
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

# ==============================================================================
# Libraries setup
# ==============================================================================
# Find and setup Qt4.
set(CTRL_PTS_QT_COMPONENTS Core Gui Widgets)
FIND_PACKAGE(Qt5 REQUIRED ${CTRL_PTS_QT_COMPONENTS})
#INCLUDE(${QT_USE_FILE})
include_directories(.)
# ==============================================================================
# List all source files here.
# ==============================================================================

# All Source Files here
SET(SourceFiles
  main.cpp
  mainLayout.cpp
  GraphicsMainWindow.cpp
  ControlPointTableView.cpp
  node.cpp)

# All non moc header files here
SET(HeaderFiles
  mainLayout.hpp
  GraphicsMainWindow.hpp
  node.hpp
  document.hpp)

# ==============================================================================
# Qt4 specific moc source files generation files
# ==============================================================================
# After this call, moc source files will be generated.
#QT4_AUTOMOC(${SourceFiles})

# Call this instruction if you don't include moc files.
QT5_WRAP_CPP(MocSourceFiles ${HeaderFiles})

# ==============================================================================
# Add the following files, modules and libraries to the project.
# ==============================================================================
# Files
ADD_EXECUTABLE(${ProjectName} 
  ${SourceFiles}
  ${HeaderFiles}
  ${MocSourceFiles})

if(WIN32)
  set(QT5_BASE_DIR "${_qt5Core_install_prefix}")
  foreach(component ${CTRL_PTS_QT_COMPONENTS})
    # Install Qt5 Dlls
    set(component_DLL_RELEASE ${QT5_BASE_DIR}/bin/Qt5${component}.dll)
    set(component_DLL_DEBUG ${QT5_BASE_DIR}/bin/Qt5${component}d.dll)
    install(FILES ${component_DLL_RELEASE}  DESTINATION bin/  CONFIGURATIONS Release)
    install(FILES ${component_DLL_DEBUG}    DESTINATION bin/  CONFIGURATIONS Debug)

    # copy Qt5 dlls
    file(COPY ${component_DLL_RELEASE}  DESTINATION ${EXECUTABLE_OUTPUT_PATH}/Release)
    file(COPY ${component_DLL_DEBUG}    DESTINATION ${EXECUTABLE_OUTPUT_PATH}/Debug)

    # add link lib
    list(APPEND QT_LIBRARIES Qt5::${component})
  endforeach()
endif(WIN32)

# Linking with Qt Libraries.
TARGET_LINK_LIBRARIES(${ProjectName}
  ${QT_LIBRARIES}
  ${OpenMVG_LIBRARIES}
)

SET_PROPERTY(TARGET ${ProjectName} PROPERTY FOLDER OpenMVG/software/ui)
INSTALL(TARGETS ${ProjectName} DESTINATION bin/)

