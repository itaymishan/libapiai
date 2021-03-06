INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/../apiai/src)

add_executable(text_request_example text_request.cpp)
add_executable(voice_activity_detection_example voice_activity_detection.cpp)
add_executable(voice_request_example voice_request.cpp)

file(GLOB TEST_FILES *.txt)

install (FILES ${TEST_FILES} DESTINATION ${RUNTIME_INSTALL_DIR})

if(BUILD_SHARED_LIBS)
    target_link_libraries(text_request_example apiai)
    target_link_libraries(voice_request_example apiai)
    target_link_libraries(voice_activity_detection_example apiai)
else(BUILD_SHARED_LIBS)
    target_link_libraries(text_request_example apiai_static)
    target_link_libraries(voice_request_example apiai_static)
    target_link_libraries(voice_activity_detection_example apiai_static)
endif()

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/test.txt ${CMAKE_CURRENT_BINARY_DIR}/test.txt COPYONLY)

find_package(Portaudio)

if(PORTAUDIO_FOUND)
    include_directories(${PORTAUDIO_INCLUDE_DIRS})
    add_executable(portaudio_example portaudio_example.cpp)
    target_link_libraries(portaudio_example ${PORTAUDIO_LIBRARIES})

    if(BUILD_SHARED_LIBS)
        target_link_libraries(portaudio_example apiai)
    else(BUILD_SHARED_LIBS)
        target_link_libraries(portaudio_example apiai_static)
    endif()
else(PORTAUDIO_FOUND)
    message(WARNING "Not found PORTAUDIO. Example could not be compiled. It will be passed.")
endif(PORTAUDIO_FOUND)

find_package (Threads)
target_link_libraries (voice_request_example ${CMAKE_THREAD_LIBS_INIT})
