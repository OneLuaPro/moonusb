# MIT License
#
# Copyright (c) 2023 Kritzel Kratzel.
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of
# this software and associated documentation files (the "Software"), to deal in 
# the Software without restriction, including without limitation the rights to 
# use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
# the Software, and to permit persons to whom the Software is furnished to do so,
# subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all 
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

# ------------------------------------------------------------------------------
# moonusb DLL
add_library(moonusb SHARED)
# setup lua include directory
target_include_directories(moonusb PRIVATE ${LIBLUA_INCLUDEDIR})
# setup platform-specific sources, compile and linker options
if(WIN32 AND NOT MinGW)
  target_compile_options(moonusb PRIVATE /D_WIN32 /D_CRT_SECURE_NO_WARNINGS)
  target_link_options(moonusb PRIVATE /LIBPATH:${LIBLUA_LIBDIR} liblua.lib libusb.lib)
else()
  message(FATAL_ERROR "Not yet fully implemented.")
endif()
# sources
target_sources(moonusb
  PRIVATE
  compat-5.3.h enums.h internal.h objects.h tree.h udata.h
  compat-5.3.c context.c datahandling.c datastructs.c devhandle.c device.c enums.c
  hostmem.c hotplug.c interface.c main.c objects.c polling.c synch.c tracing.c
  transfer.c udata.c utils.c
  PUBLIC FILE_SET HEADERS
  FILES moonusb.h
)

# ------------------------------------------------------------------------------
# Installation instructions
install(
  TARGETS moonusb FILE_SET HEADERS
  RUNTIME DESTINATION ${INSTALL_TOP_CDIR}
  PUBLIC_HEADER DESTINATION ${INSTALL_INCLUDEDIR}
)

# ------------------------------------------------------------------------------
# install lua files to defined locations
install(
  FILES
  ${CMAKE_SOURCE_DIR}/moonusb/bosdescriptors.lua
  ${CMAKE_SOURCE_DIR}/moonusb/emulator.lua
  ${CMAKE_SOURCE_DIR}/moonusb/utils.lua
  DESTINATION ${INSTALL_MOONUSB_LDIR}
)
