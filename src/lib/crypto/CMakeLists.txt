project(softhsm_crypto)

set(INCLUDE_DIRS ${PROJECT_SOURCE_DIR}
                 ${PROJECT_SOURCE_DIR}/../common
                 ${PROJECT_SOURCE_DIR}/../data_mgr
                 ${PROJECT_SOURCE_DIR}/../pkcs11
                 ${CRYPTO_INCLUDES}
                 )

set(SOURCES AESKey.cpp
            AsymmetricAlgorithm.cpp
            AsymmetricKeyPair.cpp
            CryptoFactory.cpp
            DerUtil.cpp
            DESKey.cpp
            DHParameters.cpp
            DHPrivateKey.cpp
            DHPublicKey.cpp
            DSAParameters.cpp
            DSAPrivateKey.cpp
            DSAPublicKey.cpp
            ECParameters.cpp
            ECPrivateKey.cpp
            ECPublicKey.cpp
            EDPrivateKey.cpp
            EDPublicKey.cpp
            GOSTPrivateKey.cpp
            GOSTPublicKey.cpp
            HashAlgorithm.cpp
            MacAlgorithm.cpp
            RSAParameters.cpp
            RSAPrivateKey.cpp
            RSAPublicKey.cpp
            SymmetricAlgorithm.cpp
            SymmetricKey.cpp
            )

if(WITH_OPENSSL)
    list(APPEND SOURCES OSSLAES.cpp
                        OSSLCMAC.cpp
                        OSSLComp.cpp
                        OSSLCryptoFactory.cpp
                        OSSLDES.cpp
                        OSSLDH.cpp
                        OSSLDHKeyPair.cpp
                        OSSLDHPrivateKey.cpp
                        OSSLDHPublicKey.cpp
                        OSSLDSA.cpp
                        OSSLDSAKeyPair.cpp
                        OSSLDSAPrivateKey.cpp
                        OSSLDSAPublicKey.cpp
                        OSSLECDH.cpp
                        OSSLECDSA.cpp
                        OSSLECKeyPair.cpp
                        OSSLECPrivateKey.cpp
                        OSSLECPublicKey.cpp
                        OSSLEDDSA.cpp
                        OSSLEDKeyPair.cpp
                        OSSLEDPrivateKey.cpp
                        OSSLEDPublicKey.cpp
                        OSSLEVPCMacAlgorithm.cpp
                        OSSLEVPHashAlgorithm.cpp
                        OSSLEVPMacAlgorithm.cpp
                        OSSLEVPSymmetricAlgorithm.cpp
                        OSSLGOST.cpp
                        OSSLGOSTKeyPair.cpp
                        OSSLGOSTPrivateKey.cpp
                        OSSLGOSTPublicKey.cpp
                        OSSLGOSTR3411.cpp
                        OSSLHMAC.cpp
                        OSSLMD5.cpp
                        OSSLRNG.cpp
                        OSSLRSA.cpp
                        OSSLRSAKeyPair.cpp
                        OSSLRSAPrivateKey.cpp
                        OSSLRSAPublicKey.cpp
                        OSSLSHA1.cpp
                        OSSLSHA224.cpp
                        OSSLSHA256.cpp
                        OSSLSHA384.cpp
                        OSSLSHA512.cpp
                        OSSLUtil.cpp
                        )
endif(WITH_OPENSSL)

if(WITH_BOTAN)
    list(APPEND SOURCES BotanAES.cpp
                        BotanCryptoFactory.cpp
                        BotanDES.cpp
                        BotanDH.cpp
                        BotanDHKeyPair.cpp
                        BotanDHPrivateKey.cpp
                        BotanDHPublicKey.cpp
                        BotanDSA.cpp
                        BotanDSAKeyPair.cpp
                        BotanDSAPrivateKey.cpp
                        BotanDSAPublicKey.cpp
                        Botan_ecb.cpp
                        BotanECDH.cpp
                        BotanECDHKeyPair.cpp
                        BotanECDHPrivateKey.cpp
                        BotanECDHPublicKey.cpp
                        BotanECDSA.cpp
                        BotanECDSAKeyPair.cpp
                        BotanECDSAPrivateKey.cpp
                        BotanECDSAPublicKey.cpp
                        BotanEDDSA.cpp
                        BotanEDKeyPair.cpp
                        BotanEDPrivateKey.cpp
                        BotanEDPublicKey.cpp
                        BotanGOST.cpp
                        BotanGOSTKeyPair.cpp
                        BotanGOSTPrivateKey.cpp
                        BotanGOSTPublicKey.cpp
                        BotanGOSTR3411.cpp
                        BotanHashAlgorithm.cpp
                        BotanMacAlgorithm.cpp
                        BotanMAC.cpp
                        BotanMD5.cpp
                        BotanRNG.cpp
                        BotanRSA.cpp
                        BotanRSAKeyPair.cpp
                        BotanRSAPrivateKey.cpp
                        BotanRSAPublicKey.cpp
                        BotanSHA1.cpp
                        BotanSHA224.cpp
                        BotanSHA256.cpp
                        BotanSHA384.cpp
                        BotanSHA512.cpp
                        BotanSymmetricAlgorithm.cpp
                        BotanUtil.cpp
                        )
endif(WITH_BOTAN)

if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
	list(APPEND INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/../win32)
ENDIF()

if(WITH_BOTAN)
	# mute botan specific warnings
	# https://github.com/randombit/botan/issues/486
	list(APPEND COMPILE_OPTIONS "/wd4250;/wd4251;/wd4275;/wd4127;/wd4273") 
endif()

include_directories(${INCLUDE_DIRS})
add_library(${PROJECT_NAME} OBJECT ${SOURCES})
target_compile_options(${PROJECT_NAME} PRIVATE ${COMPILE_OPTIONS})

# Versions before CMake 3.12 cannot use target_link_libraries on object
# libraries, a workaround exists in src/lib/CMakeLists.txt.
if(NOT CMAKE_VERSION VERSION_LESS "3.12")
        target_link_libraries(${PROJECT_NAME} ${CRYPTO_LIBS})
endif()

if(BUILD_TESTS)
    add_subdirectory(test)
endif(BUILD_TESTS)
