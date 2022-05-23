cmake_minimum_required(VERSION 2.8)

include_directories(
  ${GSTREAMER_INCLUDE_DIRS}
  ${CMAKE_CURRENT_SOURCE_DIR}
)

set(GST_DTLS_SOURCES
  gstdtlsagent.c
  gstdtlscertificate.c
  gstdtlsconnection.c
  gstdtlsdec.c
  gstdtlsenc.c
  gstdtlssrtpbin.c
  gstdtlssrtpdec.c
  gstdtlssrtpdemux.c
  gstdtlssrtpenc.c
  plugin.c
)

set(GST_DTLS_HEADERS
  gstdtlsagent.h
  gstdtlscertificate.h
  gstdtlsconnection.h
  gstdtlsdec.h
  gstdtlsenc.h
  gstdtlssrtpbin.h
  gstdtlssrtpdec.h
  gstdtlssrtpdemux.h
  gstdtlssrtpenc.h
)

add_library(gstdtls MODULE ${GST_DTLS_SOURCES})
if(SANITIZERS_ENABLED)
  add_sanitizers(gstdtls)
endif()


set_property(TARGET gstdtls
  PROPERTY INCLUDE_DIRECTORIES
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}/../../..
    ${KmsGstCommons_INCLUDE_DIRS}
    ${gstreamer-1.5_INCLUDE_DIRS}
)

target_link_libraries(gstdtls
  ${gstreamer-1.5_LIBRARIES}
  ${gstreamer-base-1.5_LIBRARIES}
)

install(
  TARGETS gstdtls
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_GST_PLUGINS_DIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
