cmake_minimum_required(VERSION 3.15)
set(CMAKE_OSX_DEPLOYMENT_TARGET "10.12" CACHE STRING "Minimum OS X deployment target")
project(CookyTape VERSION 0.8.6)

set(CMAKE_CXX_STANDARD 17)
add_subdirectory(modules/JUCE)

# Set your choice of RTNeural backends here!
set(RTNEURAL_STL ON CACHE BOOL "Use RTNeural with this backend" FORCE)
add_subdirectory(modules/RTNeural)
include_directories(modules/RTNeural)

juce_add_plugin(RTNeuralExample
    COMPANY_NAME DigablePla.net
    PLUGIN_MANUFACTURER_CODE COOK
    PLUGIN_CODE RTN1
    FORMATS AU VST3 Standalone
    ProductName "CookyTape"

    VST3_CATEGORIES Fx Distortion

    IS_SYNTH FALSE
    NEEDS_MIDI_INPUT FALSE
    NEEDS_MIDI_OUTPUT FALSE
    IS_MIDI_EFFECT FALSE
    EDITOR_WANTS_KEYBOARD_FOCUS FALSE
)

# create JUCE header
juce_generate_juce_header(RTNeuralExample)

# add sources
target_sources(RTNeuralExample PRIVATE
    plugin/Plugin.cpp
)

juce_add_binary_data(BinaryData SOURCES
    neural_net_weights.json
)

# add
option(UniversalBinary "Build universal binary for mac" OFF)

if (UniversalBinary)
    set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64" CACHE INTERNAL "")
    message(STATUS "Building Universal Binary")
endif ()

if (APPLE)
    set(COPY_PLUGIN_AFTER_BUILD TRUE)
else ()
    set(COPY_PLUGIN_AFTER_BUILD FALSE)
endif ()
# add_fin


# Need to build BinaryData with -fPIC flag on Linux
set_target_properties(BinaryData PROPERTIES
    POSITION_INDEPENDENT_CODE TRUE)
# set_target_properties (myPlugin_AAX PROPERTIES OSX_ARCHITECTURES x86_64)

target_compile_definitions(RTNeuralExample
    PUBLIC
    JUCE_DISPLAY_SPLASH_SCREEN=0
    JUCE_REPORT_APP_USAGE=0
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_VST3_CAN_REPLACE_VST2=0
    JUCE_MODAL_LOOPS_PERMITTED=1
)

target_link_libraries(RTNeuralExample PUBLIC
    BinaryData
    juce::juce_audio_utils
    juce::juce_dsp
    RTNeural
)
