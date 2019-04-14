PROJECT(Main LANGUAGES CSharp)
CMAKE_MINIMUM_REQUIRED(VERSION 3.12)
set(CMAKE_DOTNET_TARGET_FRAMEWORK_VERSION "4.5.2")

add_executable(Main GCode.cs CommandType.cs Program.cs Tests.cs Command.cs Properties/AssemblyInfo.cs App.config)

add_dependencies(Main Firmware Hardware)

add_custom_command(TARGET Main
	PRE_BUILD
	COMMAND nuget install OpenTK -version 2.0.0 -OutputDirectory ${CMAKE_BINARY_DIR}
)

set(CMAKE_CSharp_FLAGS "/langversion:7")

include(ExternalProject)
ExternalProject_Add(Hardware
		    PREFIX ${CMAKE_BINARY_DIR}
		    GIT_REPOSITORY
"https://github.com/dantmw/3DPrinterSIMHardware.git"
	 	    PATCH_COMMAND ""
		    BINARY_DIR ${CMAKE_BINARY_DIR}/Hardware
		    SOURCE_DIR ${CMAKE_BINARY_DIR}/Hardware
                    INSTALL_DIR ${CMAKE_BINARY_DIR}/Hardware
                                        CONFIGURE_COMMAND ""
                                        BUILD_COMMAND ""
                                        INSTALL_COMMAND ""
                                        )
include(ExternalProject)
ExternalProject_Add(Firmware
		    PREFIX ${CMAKE_BINARY_DIR}
		    GIT_REPOSITORY
"https://github.com/dakotacookenmaster/CPTR302-Firmware.git"
	 	    PATCH_COMMAND ""
		    BINARY_DIR ${CMAKE_BINARY_DIR}/Firmware
		    SOURCE_DIR ${CMAKE_BINARY_DIR}/Firmware
                    INSTALL_DIR ${CMAKE_BINARY_DIR}/Firmware
                                        CONFIGURE_COMMAND ""
                                        BUILD_COMMAND ""
                                        INSTALL_COMMAND ""
                                        )

set_property(TARGET Main PROPERTY VS_DOTNET_REFERENCES "System.Drawing" "OpenTK.2.0.0/lib/net20/OpenTK.dll" "Hardware/bin/Release/Hardware.dll" "Firmware/Release/Firmware.dll" "system")
