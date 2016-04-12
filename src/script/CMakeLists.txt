set(BITCOIN-CXX-SOURCES
        ${CMAKE_CURRENT_SOURCE_DIR}/bitcoinconsensus.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/interpreter.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/script.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/script_error.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/sigcache.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/sign.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/standard.cpp
        PARENT_SCOPE
        )

set(LIBBITCOIN-SERVER-CXX-SOURCES
        ${CMAKE_CURRENT_SOURCE_DIR}/sigcache.cpp
        PARENT_SCOPE
        )