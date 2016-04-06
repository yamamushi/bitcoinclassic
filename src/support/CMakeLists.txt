add_subdirectory(allocators)

set(BITCOIN-CXX-SOURCES
        ${CMAKE_CURRENT_SOURCE_DIR}/cleanse.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/pagelocker.cpp
        PARENT_SCOPE
        )