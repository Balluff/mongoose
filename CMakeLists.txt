cmake_minimum_required (VERSION 3.14.7)

project(mongoose)

set(MONGOOSE_SOURCES
    ${CMAKE_CURRENT_LIST_DIR}/mongoose.c
    CACHE INTERNAL "List of all mongoose sources"
)

set(MONGOOSE_INCLUDES
    ${CMAKE_CURRENT_LIST_DIR}/mongoose.h
    CACHE INTERNAL "List of all mongoose sources"
)

set(MONGOOSE_INCLUDE_DIRECTORIES
    ${CMAKE_CURRENT_LIST_DIR}
    CACHE INTERNAL "List of all mongoose include directories"
)



function(configure_mongoose_second_step TARGET_EXTENSION CUSTOM_BUILD_FILE_DIR DEPENDENCIES)
    
    if(TARGET_EXTENSION STREQUAL "")
        set(TARGET_NAME mongoose)
    else()
        set(TARGET_NAME mongoose-${TARGET_EXTENSION})
    endif()

    add_library(${TARGET_NAME}
        EXCLUDE_FROM_ALL
        ${MONGOOSE_SOURCES}
        ${MONGOOSE_INCLUDES}
        ${CUSTOM_BUILD_FILE_DIR}/mongoose_custom.cpp
        ${CUSTOM_BUILD_FILE_DIR}/mongoose_custom.h
    )
    
    target_compile_definitions(${TARGET_NAME} PUBLIC MG_ARCH=MG_ARCH_CUSTOM)
    target_compile_options(${TARGET_NAME} PRIVATE -include "mongooseCustomConfig.h")
    
    target_include_directories(${TARGET_NAME} PUBLIC
                                ${MONGOOSE_INCLUDE_DIRECTORIES}
                                ${CUSTOM_BUILD_FILE_DIR}
    )   

    target_link_libraries(${TARGET_NAME} PUBLIC
                          ${DEPENDENCIES} 
    )
    
endfunction()

