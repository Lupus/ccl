cmake_minimum_required(VERSION 2.6)
project(ccl C)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

if(NOT CMAKE_BUILD_TYPE)
	message(STATUS "No build type selected, defaulting to RelWithDebInfo")
	set(CMAKE_BUILD_TYPE "RelWithDebInfo")
endif(NOT CMAKE_BUILD_TYPE)

execute_process(COMMAND sh -c "head -1 debian/changelog | grep -o -E '\\([^)]+' | cut -b2-"
	WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
	OUTPUT_VARIABLE DEB_VERSION
	OUTPUT_STRIP_TRAILING_WHITESPACE
)

string(REGEX MATCHALL "[^.]+" DEB_VERSION_LIST "${DEB_VERSION}")
list(GET DEB_VERSION_LIST 0 VERSION_MAJOR)
list(GET DEB_VERSION_LIST 1 VERSION_MINOR)
list(GET DEB_VERSION_LIST 2 VERSION_PATCH)
set(VERSION_STRING ${DEB_VERSION})

set(CMAKE_C_FLAGS "-Wall -Wno-pointer-sign -Werror")
add_definitions(-DUNIX)

set(CCL_SOURCES
	"vector.c"
	"error.c"
	"dlist.c"
	"qsortex.c"
	"bitstrings.c"
	"generic.c"
	"dictionary.c"
	"wdictionary.c"
	"list.c"
	"strcollection.c"
	"searchtree.c"
	"heap.c"
	"malloc_debug.c"
	"bloom.c"
	"fgetline.c"
	"pool.c"
	"pooldebug.c"
	"redblacktree.c"
	"scapegoat.c"
	"queue.c"
	"buffer.c"
	"observer.c"
	"valarraydouble.c"
	"valarrayint.c"
	"vectorsize_t.c"
	"valarraylongdouble.c"
	"valarrayshort.c"
	"valarrayfloat.c"
	"valarrayuint.c"
	"valarraylonglong.c"
	"valarrayulonglong.c"
	"memorymanager.c"
	"sequential.c"
	"iMask.c"
	"deque.c"
	"hashtable.c"
	"wstrcollection.c"
	"stringlist.c"
	"wstringlist.c"
	"priorityqueue.c"
	"intlist.c"
	"doublelist.c"
	"longlonglist.c"
	"intdlist.c"
	"doubledlist.c"
	"longlongdlist.c"
	)

set(CCL_HEADERS
	"containers.h"
	"dlistgen.h"
	"doubledlist.h"
	"doublelist.h"
	"intdlist.h"
	"intlist.h"
	"listgen.h"
	"longlongdlist.h"
	"longlonglist.h"
	"stringlistgen.h"
	"stringlist.h"
	"valarraygen.h"
	"valarray.h"
	"vectorgen.h"
	"wstringlist.h"
	)

add_library(ccl SHARED ${CCL_SOURCES})
set_target_properties(ccl
	PROPERTIES
	SOVERSION ${VERSION_MAJOR}
	VERSION ${VERSION_STRING}
	)

install(TARGETS ccl
	RUNTIME DESTINATION bin
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
	)

install(FILES ${CCL_HEADERS} DESTINATION "include/ccl")
