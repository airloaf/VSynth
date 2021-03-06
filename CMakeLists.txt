cmake_minimum_required(VERSION 3.9)

# Library locations (Change these to the appropriate path on your machine)
set(INCLUDE_DIR C:/DevelopmentLibraries/include/)
set(LIB_DIR C:/DevelopmentLibraries/lib/x64)

set(LIB_TYPE STATIC)

# Create a project for VSSynth
project(VSSynth_project)

file(GLOB_RECURSE VSYNTH_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} src/*.cpp)
add_library(VSSynth ${LIB_TYPE} ${VSYNTH_SRCS})
target_include_directories(VSSynth PUBLIC include)

# Set include and link directories
target_include_directories(VSSynth PUBLIC ${INCLUDE_DIR})
target_link_directories(VSSynth PUBLIC ${LIB_DIR})

# Link Libraries for SDL2
target_link_libraries(VSSynth PUBLIC SDL2)

# Tone Example
file(GLOB_RECURSE SIMPLE_TONE_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} examples/SimpleTone/src/*.cpp)
add_executable(SimpleTone ${SIMPLE_TONE_SRCS})
target_link_libraries(SimpleTone SDL2)
target_link_libraries(SimpleTone SDL2main)
target_link_libraries(SimpleTone VSSynth)

target_include_directories(SimpleTone PUBLIC include)

# LFO Example
file(GLOB_RECURSE LFO_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} examples/LowFrequencyOscillator/src/*.cpp)
add_executable(LFO ${LFO_SRCS})
target_link_libraries(LFO SDL2)
target_link_libraries(LFO SDL2main)
target_link_libraries(LFO VSSynth)

target_include_directories(LFO PUBLIC include)

# Record Tone Example
file(GLOB_RECURSE RECORD_TONE_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} examples/RecordTone/src/*.cpp)
add_executable(RecordTone ${RECORD_TONE_SRCS})
target_link_libraries(RecordTone SDL2)
target_link_libraries(RecordTone SDL2main)
target_link_libraries(RecordTone VSSynth)

target_include_directories(RecordTone PUBLIC include)

# Piano Example
file(GLOB_RECURSE PIANO_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} examples/piano/src/*.cpp)
add_executable(Piano ${PIANO_SRCS})
target_link_libraries(Piano SDL2)
target_link_libraries(Piano SDL2_image)
target_link_libraries(Piano SDL2main)
target_link_libraries(Piano VSSynth)

target_include_directories(Piano PUBLIC include)

# Sequenced Beat Example
file(GLOB_RECURSE SEQ_BEAT_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} examples/SequencedBeat/src/*.cpp)
add_executable(SeqBeat ${SEQ_BEAT_SRCS})
target_link_libraries(SeqBeat SDL2)
target_link_libraries(SeqBeat SDL2_image)
target_link_libraries(SeqBeat SDL2main)
target_link_libraries(SeqBeat VSSynth)

target_include_directories(SeqBeat PUBLIC include)

# MIDI Example
file(GLOB_RECURSE MIDI_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} examples/midi/src/*.cpp)
add_executable(MIDI ${MIDI_SRCS})
target_link_libraries(MIDI SDL2)
target_link_libraries(MIDI SDL2main)
target_link_libraries(MIDI VSSynth)
target_link_libraries(MIDI midifile)

target_include_directories(MIDI PUBLIC include)

# Set include and link directories
target_include_directories(MIDI PUBLIC ${INCLUDE_DIR})
target_link_directories(MIDI PUBLIC ${LIB_DIR})

# Doxygen Docs
# Look for package, Doxygen
find_package(Doxygen)
# If we have doxygen installed, generate documentation
if(DOXYGEN_FOUND)
    # Set the input and cmake doxygen files
    set(DOXYFILE_IN ${CMAKE_CURRENT_SOURCE_DIR}/docs/Doxyfile)
    set(DOXYFILE_CMAKE ${CMAKE_CURRENT_SOURCE_DIR}/docs/Doxyfile_cmake)

    # Make a copy of the input doxygen file. CMake will generate a custom doxyfile
    configure_file(${DOXYFILE_IN} ${DOXYFILE_CMAKE} @ONLY)

    # Command to generate the documentation
    add_custom_target(documentation 
        ${DOXYGEN_EXECUTABLE} ${DOXYFILE_CMAKE}
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/docs)
endif()
