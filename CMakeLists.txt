cmake_minimum_required(VERSION 3.20)
project(ImHexPlugin)

if (NOT DEFINED ENV{IMHEX_SDK_PATH})
    message(FATAL_ERROR "The IMHEX_SDK_PATH environment variable is not set")
elseif (NOT EXISTS $ENV{IMHEX_SDK_PATH})
    message(FATAL_ERROR "The IMHEX_SDK_PATH environment variable doesn't contain a valid path")
endif()
add_subdirectory($ENV{IMHEX_SDK_PATH} ImHexSDK)

set(ARCH x86_64)
add_subdirectory(lib/discord-game-sdk)

add_imhex_plugin(
    NAME
        discord_rpc
    SOURCES
        source/discord_rpc_plugin.cpp
    LIBRARIES
        discord_game_sdk
        nlohmann_json
)

target_link_options(discord_rpc PRIVATE -undefined dynamic_lookup -flat_namespace)
