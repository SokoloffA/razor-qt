set(PROJECT razor-about)
project(${PROJECT})


set(H_FILES
)

set(MOC_FILES
)

set(CPP_FILES
    main.cpp
)


set(UI_FILES
)

set(QRC_FILES

)

set(LIBRARIES
    razorqt
    qtxdg
)

file(GLOB DESKTOP_FILES_IN
    resources/*.desktop.in
)

# Translations **********************************
include(RazorTranslate)
razor_translate(QM_FILES)
razor_translate_desktop(DESKTOP_FILES ${DESKTOP_FILES_IN} )
#************************************************
cmake_minimum_required( VERSION 2.6 )

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_SOURCE_DIR}/libraries/
)


if(NOT CMAKE_BUILD_TYPE)
    set( CMAKE_BUILD_TYPE Release )
endif (NOT CMAKE_BUILD_TYPE)

add_definitions(-Wall)
find_package(Qt4 REQUIRED)
include(${QT_USE_FILE})


set(RAZOR_SHARE_DIR ${CMAKE_INSTALL_PREFIX}/share/razor)
set(APP_SHARE_DIR ${RAZOR_SHARE_DIR}/${PROJECT})

#************************************************


qt4_wrap_cpp(MOC_SOURCES ${MOC_FILES})
qt4_wrap_ui(UI_HEADERS ${UI_FILES})
qt4_add_resources(QRC_SOURCES ${QRC_FILES})

add_executable(${PROJECT} ${CPP_FILES} ${UI_FILES} ${RESOURCES} ${QRC_SOURCES} ${QM_FILES} ${MOC_SOURCES} ${DESKTOP_FILES})
add_dependencies(${PROJECT} razorqt)
target_link_libraries(${PROJECT}  ${LIBRARIES} ${QT_LIBRARIES})

install(TARGETS ${PROJECT} RUNTIME DESTINATION bin)
install(FILES   ${CONFIG_FILES}    DESTINATION ${APP_SHARE_DIR})
install(FILES   ${DESKTOP_FILES}   DESTINATION share/applications)
