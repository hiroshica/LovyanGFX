if(NOT LOVYANGFX)
    set(LOVYANGFX "ESP-IDF")
endif()



if(${LOVYANGFX} STREQUAL "ESP-IDF")

    set(COMPONENT_ADD_INCLUDEDIRS
        src
        )
    file(GLOB SRCS
        src/lgfx/Fonts/efont/*.c
        src/lgfx/Fonts/IPA/*.c
        src/lgfx/utility/*.c
        src/lgfx/v0/*.cpp
        src/lgfx/v0/panel/*.cpp
        src/lgfx/v0/platforms/*.cpp
        src/lgfx/v0/touch/*.cpp
        src/lgfx/v1/*.cpp
        src/lgfx/v1/misc/*.cpp
        src/lgfx/v1/panel/*.cpp
        src/lgfx/v1/platforms/arduino_default/*.cpp
        src/lgfx/v1/platforms/esp32/*.cpp
        src/lgfx/v1/platforms/esp32c3/*.cpp
        src/lgfx/v1/platforms/esp32s2/*.cpp
        src/lgfx/v1/platforms/esp32s3/*.cpp
        src/lgfx/v1/touch/*.cpp
        )

    set(COMPONENT_SRCS ${SRCS})

    if (IDF_VERSION_MAJOR GREATER_EQUAL 5)
        set(COMPONENT_REQUIRES nvs_flash efuse esp_lcd driver esp_timer)
    elseif ((IDF_VERSION_MAJOR EQUAL 4) AND (IDF_VERSION_MINOR GREATER 3) OR IDF_VERSION_MAJOR GREATER 4)
        set(COMPONENT_REQUIRES nvs_flash efuse esp_lcd)
    else()
        set(COMPONENT_REQUIRES nvs_flash efuse)
    endif()


    ### If you use arduino-esp32 components, please activate next comment line.
    # list(APPEND COMPONENT_REQUIRES arduino-esp32)


    message(STATUS "LovyanGFX use components = ${COMPONENT_REQUIRES}")

    register_component()
elseif(${LOVYANGFX} STREQUAL "PICO-SDK")
    message(STATUS "LovyanGFX is build pico-sdk")

    add_definitions(-DPICO_SDK_PLATFORM)

    file(GLOB SRCS
    src/lgfx/Fonts/efont/*.c
    src/lgfx/Fonts/IPA/*.c
    src/lgfx/utility/*.c
    src/lgfx/v0/*.cpp
    src/lgfx/v0/panel/*.cpp
    src/lgfx/v0/platforms/*.cpp
    src/lgfx/v0/touch/*.cpp
    src/lgfx/v1/*.cpp
    src/lgfx/v1/misc/*.cpp
    src/lgfx/v1/panel/*.cpp
    src/lgfx/v1/platforms/pico-sdk/*.cpp
    src/lgfx/v1/touch/*.cpp
    )

    set(TARGET "LovyanGFX")

    add_library(${TARGET} INTERFACE)
    target_sources(${TARGET} INTERFACE ${SRCS})
    target_include_directories(${TARGET} INTERFACE src)
    target_link_libraries(${TARGET} INTERFACE
        pico_stdlib
        hardware_i2c
        hardware_spi
    )

else()
message(STATUS "LovyanGFX is not build")
endif()

