cmake_minimum_required(VERSION 2.8)

project(LEVELDB)
set(PROJECT_NAME LEVELDB)
set(NAME LEVELDB)

#Fix RPATH
set(CMAKE_MACOSX_RPATH 1)

set(CMAKE_INCLUDE_CURRENT_DIR TRUE)

include_directories(SYSTEM
        ${PROJECT_SOURCE_DIR}
        ${PROJECT_SOURCE_DIR}/db
        ${PROJECT_SOURCE_DIR}/include
        )

add_subdirectory(db)
add_subdirectory(helpers)
add_subdirectory(include)
add_subdirectory(port)
add_subdirectory(table)
add_subdirectory(util)

if(CMAKE_COMPILER_IS_GNUCXX)
  set(COMMON_FLAGS "${COMMON_FLAGS} -fno-builtin-memcmp")
endif()

if(CMAKE_SYSTEM_NAME matches CYGWIN_*)
  set(PLATFORM OS_LINUX)
  set(COMMON_FLAGS "${COMMON_FLAGS} -lpthread -DOS_LINUX -DCYGWIN")
elseif(CMAKE_SYSTEM_NAME matches Darwin)
  set(PLATFORM OS_MACOSX)
  set(COMMON_FLAGS "${COMMON_FLAGS} -DOS_MACOSX")
endif()


set(LEVELDB_SRC
        db/builder.cc
        db/c.cc
        db/db_impl.cc
        db/db_iter.cc
        db/dbformat.cc
        db/dumpfile.cc
        db/filename.cc
        db/log_reader.cc
        db/log_writer.cc
        db/memtable.cc
        db/repair.cc
        db/table_cache.cc
        db/version_edit.cc
        db/version_set.cc
        db/write_batch.cc
        table/block.cc
        table/block_builder.cc
        table/filter_block.cc
        table/format.cc
        table/iterator.cc
        table/merger.cc
        table/table.cc
        table/table_builder.cc
        table/two_level_iterator.cc
        util/arena.cc
        util/bloom.cc
        util/cache.cc
        util/coding.cc
        util/comparator.cc
        util/crc32c.cc
        util/env.cc
        util/env_posix.cc
        util/env_win.cc
        util/filter_policy.cc
        util/hash.cc
        util/histogram.cc
        util/logging.cc
        util/options.cc
        util/status.cc
        port/port_posix.cc
        )
# MEMENV_SOURCES=helpers/memenv/memenv.cc

add_library(${NAME} SHARED ${LEVELDB_SRC})
add_library(${NAME}-static SHARED ${LEVELDB_SRC})