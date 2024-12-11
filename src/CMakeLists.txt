cmake_minimum_required (VERSION 3.5)

set(PROJ_AUTHOR NMM2)
set(PROJ_ARCH x86)
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
  set(PROJ_ARCH x64)
endif()


set(BA2TK_SRCS
    ba2exception.cpp
    ba2archive.cpp
  )

set(BA2TK_HDRS
  ${BA2TK_INCLUDE_DIR}/ba2tk/errorcodes.h
  ${BA2TK_INCLUDE_DIR}/ba2tk/ba2type.h
  ${BA2TK_INCLUDE_DIR}/ba2tk/ba2types.h
  ${BA2TK_INCLUDE_DIR}/ba2tk/ba2exception.h
  ${BA2TK_INCLUDE_DIR}/ba2tk/ba2archive.h
  ${BA2TK_INCLUDE_DIR}/ba2tk/dds.h
  ${BA2TK_INCLUDE_DIR}/ba2tk/semaphore.h
  )

find_package(ZLIB REQUIRED)

include_directories(common ${ZLIB_INCLUDE_DIRS} ${BA2TK_INCLUDE_DIR})

if (BUILD_SHARED)
  add_library(ba2tk SHARED ${BA2TK_SRCS})
  target_link_libraries(ba2tk 
    PRIVATE
        ${ZLIB_LIBRARIES}
  )
else()
  add_library(ba2tk STATIC ${BA2TK_SRCS})
endif()

if (NOT "${OPTIMIZE_COMPILE_FLAGS}" STREQUAL "")
  set_target_properties(ba2tk PROPERTIES COMPILE_FLAGS_RELWITHDEBINFO
                        ${OPTIMIZE_COMPILE_FLAGS})
endif()
set_target_properties(ba2tk PROPERTIES LINK_FLAGS_RELWITHDEBINFO
                      "/LARGEADDRESSAWARE ${OPTIMIZE_LINK_FLAGS}")
###############
## Installation

install(TARGETS ba2tk
  ARCHIVE DESTINATION lib
  INCLUDES DESTINATION include/ba2tk)

# Install header files
install(FILES ${BA2TK_HDRS} DESTINATION include/ba2tk)
