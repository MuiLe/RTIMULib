#////////////////////////////////////////////////////////////////////////////
#//
#//  This file is part of RTIMULib
#//
#//  Copyright (c) 2014, richards-tech
#//
#//  Permission is hereby granted, free of charge, to any person obtaining a copy of
#//  this software and associated documentation files (the "Software"), to deal in
#//  the Software without restriction, including without limitation the rights to use,
#//  copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the
#//  Software, and to permit persons to whom the Software is furnished to do so,
#//  subject to the following conditions:
#//
#//  The above copyright notice and this permission notice shall be included in all
#//  copies or substantial portions of the Software.
#//
#//  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
#//  INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
#//  PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
#//  HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
#//  OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
#//  SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

#// The cmake support was based on work by Moritz Fischer at ettus.com.
#// Original copyright notice:
#
# Copyright 2014 Ettus Research LLC
#

SET(HOSTIMUGL_SRCS
    IMUView.cpp
    RTHostIMUGL.cpp
    main.cpp)

SET(GL_SRCS
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLADSShader.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLADSTextureShader.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLComponent.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGL.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLCylinderComponent.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLDiskComponent.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLFlatShader.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLPlaneComponent.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLSphereComponent.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLTextureShader.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib/QtGLWireCubeComponent.cpp)

SET(VRIMU_SRCS
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/VRWidgetLib/VRIMUWidget.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/GL/VRWidgetLib/VRWidget.cpp)

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR})
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR})

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/GL/QtGLLib)
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/GL/VRWidgetLib)

SET(CMAKE_AUTOMOC ON)

IF(DEFINED QT5)
    FIND_PACKAGE(Qt5Widgets REQUIRED)
    FIND_PACKAGE(Qt5Core REQUIRED)
    FIND_PACKAGE(Qt5OpenGL REQUIRED)
    FIND_PACKAGE(Qt5Gui REQUIRED)
    qt5_wrap_ui(UI_HEADERS RTHostIMUGL.ui)
    qt5_add_resources(RCC_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/GL/VRWidgetLib/VRWidgetLib.qrc)
    ADD_EXECUTABLE(RTHostIMUGL
        ${HOSTIMUGL_SRCS}
        ${GL_SRCS}
        ${VRIMU_SRCS}
        ${UI_HEADERS}
        ${GL_HEADERS}
        ${RCC_HEADERS})

    TARGET_LINK_LIBRARIES(RTHostIMUGL
        RTIMULib RTHostIMUCommon RTArduLinkHost ${Qt5Gui_OPENGL_LIBRARIES} ${Qt5Core_QTMAIN_LIBRARIES})
    qt5_use_modules(RTHostIMUGL Widgets Core OpenGL)
ELSE(DEFINED QT5)
    FIND_PACKAGE(Qt4 REQUIRED)
    SET(QT_USE_QTOPENGL TRUE)
    INCLUDE(${QT_USE_FILE})
    ADD_DEFINITIONS(${QT_DEFINITIONS})
    QT4_WRAP_UI(UI_HEADERS RTHostIMUGL.ui)
    QT4_ADD_RESOURCES(RCC_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/GL/VRWidgetLib/VRWidgetLib.qrc)
    ADD_EXECUTABLE(RTHostIMUGL
        ${HOSTIMUGL_SRCS}
        ${GL_SRCS}
        ${VRIMU_SRCS}
        ${UI_HEADERS}
        ${GL_HEADERS}
        ${RCC_HEADERS})
    TARGET_LINK_LIBRARIES(RTHostIMUGL RTIMULib RTHostIMUCommon RTArduLinkHost ${QT_LIBRARIES} GL)
ENDIF(DEFINED QT5)

INSTALL(TARGETS RTHostIMUGL DESTINATION bin)

