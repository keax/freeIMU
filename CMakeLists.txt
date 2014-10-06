cmake_minimum_required(VERSION 2.6)

project("test_freeimu")

###############################################################################
# EMBEDDED
###############################################################################
set(EMBEDDED_PATH ${CMAKE_CURRENT_LIST_DIR}/embedded/)
include(${EMBEDDED_PATH}/backends/cmake/project.cmake)

set(FREEIMU_PATH ${CMAKE_CURRENT_LIST_DIR}/)

###############################################################################
# PATHS
###############################################################################
set(FREEIMU_LIBRARIES_FREEIMU_PATH ${FREEIMU_PATH}/libraries/FreeIMU/)
set(FREEIMU_LIBRARIES_I2CDEV_PATH ${FREEIMU_PATH}/libraries/I2Cdev/)
set(FREEIMU_LIBRARIES_MPU60X0_PATH ${FREEIMU_PATH}/libraries/MPU60X0/)
set(FREEIMU_LIBRARIES_MS561101BA_PATH ${FREEIMU_PATH}/libraries/MS561101BA/)
set(FREEIMU_LIBRARIES_HMC58X3_PATH ${FREEIMU_PATH}/libraries/HMC58X3/)

###############################################################################
# SOURCES
###############################################################################
set(FREEIMU_LIBRARIES_FREEIMU_SOURCES ${FREEIMU_LIBRARIES_FREEIMU_PATH}/FreeIMU.cpp ${FREEIMU_LIBRARIES_FREEIMU_PATH}/FreeIMU.h ${FREEIMU_LIBRARIES_FREEIMU_PATH}/CommunicationUtils.cpp ${FREEIMU_LIBRARIES_FREEIMU_PATH}/CommunicationUtils.h)
set(FREEIMU_LIBRARIES_I2CDEV_SOURCES ${FREEIMU_LIBRARIES_I2CDEV_PATH}/I2Cdev.cpp ${FREEIMU_LIBRARIES_I2CDEV_PATH}/I2Cdev.h)
set(FREEIMU_LIBRARIES_MPU60X0_SOURCES ${FREEIMU_LIBRARIES_MPU60X0_PATH}/MPU60X0.cpp ${FREEIMU_LIBRARIES_MPU60X0_PATH}/MPU60X0.h)
set(FREEIMU_LIBRARIES_MS561101BA_SOURCES ${FREEIMU_LIBRARIES_MS561101BA_PATH}/MS561101BA.h) #${FREEIMU_LIBRARIES_MS561101BA_PATH}/MS561101BA.cpp
set(FREEIMU_LIBRARIES_HMC58X3_SOURCES ${FREEIMU_LIBRARIES_HMC58X3_PATH}/HMC58X3.cpp ${FREEIMU_LIBRARIES_HMC58X3_PATH}/HMC58X3.h)

set (PROJECT_SOURCES	${FREEIMU_PATH}/libraries/FreeIMU/examples/FreeIMU_serial/FreeIMU_serial.ino)

set_source_files_properties(${FREEIMU_PATH}/libraries/FreeIMU/examples/FreeIMU_serial/FreeIMU_serial.ino PROPERTIES LANGUAGE "CXX" HEADER_FILE_ONLY FALSE COMPILE_FLAGS "-x c++")

build_project(freeimu_serial ${PROJECT_SOURCES})

###############################################################################
# FreeIMU
###############################################################################
build_library(freeimu ${FREEIMU_LIBRARIES_FREEIMU_SOURCES})
target_include_directories(freeimu PUBLIC ${FREEIMU_LIBRARIES_FREEIMU_PATH})
target_include_directories(freeimu PUBLIC ${FREEIMU_PATH}/libraries/DebugUtils)

#TODO: Fix CPU speed
target_compile_definitions(freeimu PUBLIC F_CPU=80000000)


###############################################################################
# I2Cdev
###############################################################################
build_library(I2Cdev ${FREEIMU_LIBRARIES_I2CDEV_SOURCES})
target_include_directories(I2Cdev PUBLIC ${FREEIMU_LIBRARIES_I2CDEV_PATH})
target_compile_definitions(I2Cdev PUBLIC ARDUINO=100)
target_compile_options(I2Cdev PUBLIC -includetypes.hpp)
target_compile_options(I2Cdev PUBLIC -includefrontend_arduino.hpp)
target_link_libraries(I2Cdev pinout frontend_arduino)

###############################################################################
# MPU60X0
###############################################################################
build_library(MPU60X0 ${FREEIMU_LIBRARIES_MPU60X0_SOURCES})
target_include_directories(MPU60X0 PUBLIC ${FREEIMU_LIBRARIES_MPU60X0_PATH})
target_link_libraries(MPU60X0 I2Cdev)

###############################################################################
# MS561101BA
###############################################################################
build_library(MS561101BA ${FREEIMU_LIBRARIES_MS561101BA_SOURCES})
target_include_directories(MS561101BA PUBLIC ${FREEIMU_LIBRARIES_MS561101BA_PATH})
target_link_libraries(MS561101BA I2Cdev)

###############################################################################
# HMC58X3
###############################################################################
build_library(HMC58X3 ${FREEIMU_LIBRARIES_HMC58X3_SOURCES})
target_include_directories(HMC58X3 PUBLIC ${FREEIMU_LIBRARIES_HMC58X3_PATH})
target_link_libraries(HMC58X3 I2Cdev)

target_link_libraries(freeimu MPU60X0 MS561101BA HMC58X3)

target_link_libraries(freeimu_serial freeimu)
