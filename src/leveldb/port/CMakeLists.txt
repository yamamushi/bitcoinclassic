add_subdirectory(win)
set(COMMON_FLAGS "${COMMON_FLAGS} -D_OS_MACOSX -D_LEVELDB_PLATFORM_POSIX")

set(LEVELDB-CXX-SRC
        ${CMAKE_CURRENT_SOURCE_DIR}/port_posix.cc
        PARENT_SCOPE
        )

set(LEVELDB-CXX-HEADERS
        ${CMAKE_CURRENT_SOURCE_DIR}/port.h
        PARENT_SCOPE
        )
