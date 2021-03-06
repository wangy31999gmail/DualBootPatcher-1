if(${MBP_BUILD_TARGET} STREQUAL android)
    if(${MBP_BUILD_TYPE} STREQUAL release)
        set(_RELEASE_SIGN_JAVA_KEYSTORE_PATH
            "${MBP_SIGN_JAVA_KEYSTORE_PATH}")
        set(_RELEASE_SIGN_JAVA_KEYSTORE_PASSWORD
            "${MBP_SIGN_JAVA_KEYSTORE_PASSWORD}")
        set(_RELEASE_SIGN_JAVA_KEY_ALIAS
            "${MBP_SIGN_JAVA_KEY_ALIAS}")
        set(_RELEASE_SIGN_JAVA_KEY_PASSWORD
            "${MBP_SIGN_JAVA_KEY_PASSWORD}")
    elseif(${MBP_BUILD_TYPE} STREQUAL ci)
        set(_CI_SIGN_JAVA_KEYSTORE_PATH
            "${MBP_SIGN_JAVA_KEYSTORE_PATH}")
        set(_CI_SIGN_JAVA_KEYSTORE_PASSWORD
            "${MBP_SIGN_JAVA_KEYSTORE_PASSWORD}")
        set(_CI_SIGN_JAVA_KEY_ALIAS
            "${MBP_SIGN_JAVA_KEY_ALIAS}")
        set(_CI_SIGN_JAVA_KEY_PASSWORD
            "${MBP_SIGN_JAVA_KEY_PASSWORD}")
    endif()

    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/build.gradle.in
        ${CMAKE_CURRENT_SOURCE_DIR}/build.gradle
        @ONLY
    )

    if(WIN32)
        set(GRADLEW_CMD ".\\gradlew.bat")
    else()
        set(GRADLEW_CMD "./gradlew")
    endif()

    if(${MBP_BUILD_TYPE} STREQUAL release)
        set(GRADLEW_ARG "assembleRelease")
    elseif(${MBP_BUILD_TYPE} STREQUAL debug)
        set(GRADLEW_ARG "assembleDebug")
    elseif(${MBP_BUILD_TYPE} STREQUAL ci)
        set(GRADLEW_ARG "assembleCi")
    endif()

    add_custom_target(
        apk ${GRADLEW_CMD} ${GRADLEW_ARG}
        DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/build.gradle
                #${CMAKE_BINARY_DIR}/assets/data-${MBP_VERSION}.tar.xz
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        VERBATIM
    )
endif()
