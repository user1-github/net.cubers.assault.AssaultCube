cmake_minimum_required(VERSION 3.5)

# set compile options
set(CMAKE_CXX_FLAGS "-O3 -fomit-frame-pointer -Wall -fsigned-char")

# Use pkg-config to configure dependencies later
find_package(PkgConfig REQUIRED)
# the dependencies required by all platforms
set(client_deps zlib sdl2 SDL2_image openal vorbisfile gl libenet)

# the client depends on almost all the source files
set(client_sources
    src/crypto.cpp
    src/audiomanager.cpp
    src/autodownload.cpp
    src/client.cpp
    src/clientgame.cpp
    src/clients2c.cpp
    src/command.cpp
    src/console.cpp
    src/docs.cpp
    src/editing.cpp
    src/entities.cpp
    src/http.cpp
    src/log.cpp
    src/main.cpp
    src/menus.cpp
    src/oggstream.cpp
    src/openal.cpp
    src/packetqueue.cpp
    src/physics.cpp
    src/protocol.cpp
    src/rendercubes.cpp
    src/rendergl.cpp
    src/renderhud.cpp
    src/rendermodel.cpp
    src/renderparticles.cpp
    src/rendertext.cpp
    src/rndmap.cpp
    src/scoreboard.cpp
    src/serverms.cpp
    src/server.cpp
    src/serverbrowser.cpp
    src/shadow.cpp
    src/soundlocation.cpp
    src/soundscheduler.cpp
    src/stream.cpp
    src/texture.cpp
    src/tools.cpp
    src/water.cpp
    src/weapon.cpp
    src/wizard.cpp
    src/world.cpp
    src/worldio.cpp
    src/worldlight.cpp
    src/worldocull.cpp
    src/worldrender.cpp
    src/zip.cpp
    src/bot/bot.cpp
    src/bot/botmanager.cpp
    src/bot/bot_ai.cpp
    src/bot/bot_util.cpp
    src/bot/bot_waypoint.cpp
    src/bot/ac_bot.cpp
    src/bot/ac_bot_ai.cpp
)

# platform specific code
if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    list(APPEND client_deps x11)
    set(BIN_SUFFIX "_linux")
endif()

# configure dependencies with pkg-config
foreach(dep IN LISTS client_deps)
    pkg_check_modules(${dep} REQUIRED ${dep})

    # add the necessary includes
    foreach(include_dir IN LISTS ${dep}_INCLUDE_DIRS)
        include_directories(${include_dir})
    endforeach(include_dir)

    # link to the libraries
    foreach(lib IN LISTS ${dep}_LIBRARIES)
        link_libraries(${lib})
    endforeach(lib)

    # tell the compiler where to find the libraries' binaries
    foreach(lib_dir IN LISTS ${dep}_LIBRARY_DIRS)
        link_directories(${lib_dir})
    endforeach(lib_dir)
endforeach(dep)

# configure local includes
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/src/bot
)

# add the client executable
add_executable(ac${BIN_SUFFIX} ${client_sources})
