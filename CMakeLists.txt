
# Need @rpath support introduced in 2.8.12
CMAKE_MINIMUM_REQUIRED(VERSION 2.8.12)


file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/source/Info.plist DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/source/AppDelegate.swift DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/source/ViewController.swift DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

# Copy the whole Folder

file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/source/Base.lproj DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/source/Assets.xcassets DESTINATION ${CMAKE_CURRENT_BINARY_DIR})


# set(CMAKE_XCODE_ATTRIBUTE_SWIFT_OBJC_BRIDGING_HEADER "${CMAKE_CURRENT_BINARY_DIR}/MySwiftBridgingHeader.h")	
# set(CMAKE_XCODE_ATTRIBUTE_SWIFT_OBJC_INTERFACE_HEADER_NAME "MySwiftInterfaceHeader.h")


IF(APPLE)
        set(CMAKE_MACOSX_RPATH 1)
        # use, i.e. don't skip the full RPATH for the build tree
        #       SET(CMAKE_SKIP_BUILD_RPATH  FALSE)

        # when building, don't use the install RPATH already
        # (but later on when installing)
        SET(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE) 

        #SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")

        # add the automatically determined parts of the RPATH
        # which point to directories outside the build tree to the install RPATH
        SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH FALSE)    
ENDIF()

# Add your C/C++/Obj-C files here
set(MY_COMPILED_FILES
	${CMAKE_CURRENT_BINARY_DIR}/Info.plist
	${CMAKE_CURRENT_BINARY_DIR}/AppDelegate.swift
	${CMAKE_CURRENT_BINARY_DIR}/ViewController.swift
		
)

set (RESOURCES
     ${CMAKE_CURRENT_BINARY_DIR}/Base.lproj/Main.storyboard
     ${CMAKE_CURRENT_BINARY_DIR}/Base.lproj/LaunchScreen.storyboard
     ${CMAKE_CURRENT_BINARY_DIR}/Assets.xcassets
)

ADD_EXECUTABLE(${DEVELOPMENT_PROJECT_NAME}
	${MY_COMPILED_FILES}
        ${RESOURCES}
)

# Create the app target
SET_TARGET_PROPERTIES(${DEVELOPMENT_PROJECT_NAME} PROPERTIES
                      XCODE_ATTRIBUTE_DEBUG_INFORMATION_FORMAT "dwarf-with-dsym"
                      XCODE_ATTRIBUTE_GCC_PREFIX_HEADER "${CMAKE_CURRENT_SOURCE_DIR}/Prefix.pch"
                      RESOURCE "${RESOURCES}"
                      XCODE_ATTRIBUTE_GCC_PRECOMPILE_PREFIX_HEADER "YES"
                      XCODE_ATTRIBUTE_IPHONEOS_DEPLOYMENT_TARGET ${DEPLOYMENT_TARGET}
                      XCODE_ATTRIBUTE_CODE_SIGN_IDENTITY ${CODE_SIGN_IDENTITY}
                      XCODE_ATTRIBUTE_DEVELOPMENT_TEAM ${DEVELOPMENT_TEAM_ID}
                      XCODE_ATTRIBUTE_TARGETED_DEVICE_FAMILY ${DEVICE_FAMILY}
                      MACOSX_BUNDLE_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/plist.in
                      XCODE_ATTRIBUTE_CLANG_ENABLE_OBJC_ARC YES
                      XCODE_ATTRIBUTE_COMBINE_HIDPI_IMAGES NO
                      XCODE_ATTRIBUTE_INSTALL_PATH "$(LOCAL_APPS_DIR)"
                      XCODE_ATTRIBUTE_ENABLE_TESTABILITY YES
                      XCODE_ATTRIBUTE_GCC_SYMBOLS_PRIVATE_EXTERN YES
)

