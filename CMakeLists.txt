# QPC Zephyr application template and example
# SPDX-License-Identifier: GPL-3.0-or-later OR LicenseRef-QL-commercial

cmake_minimum_required(VERSION 3.20.0)

# location of the QPC Zephyr module
# NOTE: might need to adjust for your project
#
# set(QPC ${CMAKE_CURRENT_SOURCE_DIR}/test/qpc)

set(QPC $ENV{HOME}/Projects/qp/qpc)

# echo $ENV{HOME}

# Zephyr stuff
set(ZEPHYR_EXTRA_MODULES ${QPC})
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})

# project name
# NOTE: Adjust for your project
#
project(qpc-zephyr-app)

list(APPEND CMAKE_MODULE_PATH ${ZEPHYR_BASE}/modules/nanopb)
include(nanopb)

if(CONFIG_SAMPLE_UNLUCKY_NUMBER)
  set(unlucky_number_type "FT_DEFAULT")
else()
  set(unlucky_number_type "FT_IGNORE")
endif()
zephyr_nanopb_sources(app src/simple.proto)
zephyr_nanopb_sources(app src/protocol.proto)

# sources for your project
# NOTE: Adjust for your project
#
target_sources(app PRIVATE
 src/main.c
 src/bsp.c
 src/philo.c
 src/table.c
)

zephyr_include_directories(
 ${QPC}/include
 ${QPC}/zephyr
 ./src
)

# option to turn QSPY software tracing ON or OFF
# NOTE: Typically specified on command-line, e.g.:
# west build -b <board> -- -DQSPY=ON
#
option(QSPY "QSPY software tracing" OFF)

