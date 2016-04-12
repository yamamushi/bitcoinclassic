set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ../bin/)

add_subdirectory(bench)
add_subdirectory(compat)
add_subdirectory(consensus)
add_subdirectory(crypto)
add_subdirectory(leveldb)
add_subdirectory(policy)
add_subdirectory(primitives)
add_subdirectory(qt)
add_subdirectory(script)
add_subdirectory(secp256k1)
add_subdirectory(support)
add_subdirectory(test)
add_subdirectory(univalue)
add_subdirectory(wallet)
add_subdirectory(zmq)

set(BITCOIN-CXX-SOURCES
        addrman.cpp
        alert.cpp
        amount.cpp
        arith_uint256.cpp
        base58.cpp
        bitcoind.cpp
        bloom.cpp
        chain.cpp
        chainparams.cpp
        chainparamsbase.cpp
        checkpoints.cpp
        clientversion.cpp
        coins.cpp
        compressor.cpp
        core_read.cpp
        core_write.cpp
        dbwrapper.cpp
        hash.cpp
        httprpc.cpp
        httpserver.cpp
        init.cpp
        key.cpp
        keystore.cpp
        main.cpp
        merkleblock.cpp
        miner.cpp
        net.cpp
        netbase.cpp
        noui.cpp
        pow.cpp
        protocol.cpp
        pubkey.cpp
        random.cpp
        rest.cpp
        rpcblockchain.cpp
        rpcclient.cpp
        rpcmining.cpp
        rpcmisc.cpp
        rpcnet.cpp
        rpcprotocol.cpp
        rpcrawtransaction.cpp
        rpcserver.cpp
        scheduler.cpp
        sync.cpp
        timedata.cpp
        torcontrol.cpp
        txdb.cpp
        txmempool.cpp
        uint256.cpp
        util.cpp
        utilmoneystr.cpp
        utilstrencodings.cpp
        utiltime.cpp
        validationinterface.cpp
        )

set(LIBBITCOIN-SERVER-CXX-SOURCES
        addrman.cpp
        alert.cpp
        bloom.cpp
        chain.cpp
        checkpoints.cpp
        httprpc.cpp
        httpserver.cpp
        init.cpp
        dbwrapper.cpp
        main.cpp
        merkleblock.cpp
        miner.cpp
        net.cpp
        noui.cpp
        pow.cpp
        rest.cpp
        rpcblockchain.cpp
        rpcmining.cpp
        rpcmisc.cpp
        rpcnet.cpp
        rpcrawtransaction.cpp
        rpcserver.cpp
        timedata.cpp
        torcontrol.cpp
        txdb.cpp
        txmempool.cpp
        validationinterface.cpp
        )

set(LIBBITCOIN-CLI-CXX-SOURCES
        bitcoin-cli.cpp
        )

#-----------------------------------------------------------------------------
# Setup our libraries

#add_library(libbitcoin_server-static STATIC ${LIBBITCOIN-SERVER-CXX-SOURCES})
#add_library(libbitcoin_server SHARED ${LIBBITCOIN-SERVER-CXX-SOURCES})
#target_link_libraries(libbitcoin_server-static )

#add_library(libbitcoin_wallet-static STATIC ${LIBBITCOIN-WALLET-CXX-SOURCES})
#add_library(libbitcoin_wallet SHARED ${LIBBITCOIN-WALLET-CXX-SOURCES})

#add_library(libbitcoin_common-static STATIC ${LIBBITCOIN-COMMON-CXX-SOURCES})
#add_library(libbitcoin_common SHARED ${LIBBITCOIN-COMMON-CXX-SOURCES})

#add_library(libbitcoin_cli-static STATIC ${LIBBITCOIN-CLI-CXX-SOURCES})
#add_library(libbitcoin_cli SHARED ${LIBBITCOIN-CLI-CXX-SOURCES})
#target_link_libraries(libbitcoin_cli-static libunivalue-static libbitcoin_cli-static)

#add_library(libbitcoin_util-static STATIC ${LIBBITCOIN-UTIL-CXX-SOURCES})
#add_library(libbitcoin_util SHARED ${LIBBITCOIN-UTIL-CXX-SOURCES})

#add_library(libbitcoin_crypto-static STATIC ${LIBBITCOIN-CRYPTO-CXX-SOURCES})
#add_library(libbitcoin_crypto SHARED ${LIBBITCOIN-CRYPTO-CXX-SOURCES})

# libsecp256k1
# libunivalue

#-----------------------------------------------------------------------------
# Setup our binaries

#add_executable(bitcoind ${BITCOIN-CXX-SOURCES})

#target_link_libraries(bitcoind ${LIBBITCOIN_SERVER} ${LIBBITCOIN_COMMON} ${LIBUNIVALUE} ${LIBBITCOIN_UTIL}
#    ${LIBBITCOIN_CRYPTO} ${LIBLEVELDB} ${LIBMEMENV} ${LIBSECP256K1}
#    )