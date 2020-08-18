cmake_minimum_required(VERSION 3.10)

# set the project name
project(Fact++ VERSION 0.0.1)


IF(IS_DIRECTORY "${PROJECT_SOURCE_DIR}/cfitsio-3.47")
	execute_process(COMMAND rm -r cfitsio-3.47 )
ENDIF()

set(VAR_OS "")
IF(CMAKE_SYSTEM_NAME MATCHES "Linux")
        execute_process (
                COMMAND bash -c "awk -F= '/^ID=/{print $2}' /etc/os-release |tr -d '\n' | tr -d '\"'"
                OUTPUT_VARIABLE outOS
        )
        MESSAGE(DEBUG "Linux os: ${outOS}") # outOS = Linux distribution
ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Windows")
        MESSAGE(DEBUG "Windows")
        set(VAR_OS "wins")
ELSE()
        MESSAGE(FATAL "No OS detected!")
ENDIF(CMAKE_SYSTEM_NAME MATCHES "Linux")

IF(outOS MATCHES "ubuntu")
	execute_process(COMMAND sh dependencies_ubuntu.sh) # download all dependencies
	execute_process(COMMAND wget http://heasarc.gsfc.nasa.gov/FTP/software/fitsio/c/cfitsio-3.47.tar.gz)
	execute_process(COMMAND tar -xvzf cfitsio-3.47.tar.gz)
	execute_process(COMMAND rm cfitsio-3.47.tar.gz)
	execute_process(COMMAND cd cfitsio-3.47)
	execute_process(COMMAND ./configure)


ELSEIF(outOS MATCHES "fedora")

ELSE()
	MESSAGE(FATAL "No OS detected!")
ENDIF(outOS MATCHES "ubuntu")



