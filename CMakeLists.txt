cmake_minimum_required(VERSION 3.5)

# set compile options
set(CMAKE_CXX_FLAGS "-O3 -fomit-frame-pointer -ffast-math -Wall -fsigned-char -rdynamic -Wno-format-zero-length -Wno-stringop-truncation")

# Use pkg-config to configure dependencies later
find_package(PkgConfig REQUIRED)
# the dependencies required by all platforms
set(client_deps zlib sdl2 SDL2_image openal vorbisfile gl libenet)

# the client depends on almost all the source files
set(client_sources
    crypto.cpp
    audiomanager.cpp
    autodownload.cpp
    client.cpp
    clientgame.cpp
    clients2c.cpp
    command.cpp
    console.cpp
    docs.cpp
    editing.cpp
    entities.cpp
    http.cpp
    log.cpp
    main.cpp
    menus.cpp
    oggstream.cpp
    openal.cpp
    packetqueue.cpp
    physics.cpp
    protocol.cpp
    rendercubes.cpp
    rendergl.cpp
    renderhud.cpp
    rendermodel.cpp
    renderparticles.cpp
    rendertext.cpp
    rndmap.cpp
    scoreboard.cpp
    serverms.cpp
    server.cpp
    serverbrowser.cpp
    shadow.cpp
    soundlocation.cpp
    soundscheduler.cpp
    stream.cpp
    texture.cpp
    tools.cpp
    water.cpp
    weapon.cpp
    wizard.cpp
    world.cpp
    worldio.cpp
    worldlight.cpp
    worldocull.cpp
    worldrender.cpp
    zip.cpp
    bot/bot.cpp
    bot/botmanager.cpp
    bot/bot_ai.cpp
    bot/bot_util.cpp
    bot/bot_waypoint.cpp
    bot/ac_bot.cpp
    bot/ac_bot_ai.cpp
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
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/bot
)

# add the client executable
add_executable(ac${BIN_SUFFIX} ${client_sources})
