cmake_minimum_required(VERSION 3.16)

project(mongoose)

add_library(mongoose
            EXCLUDE_FROM_ALL
            mongoose.c
            mongoose.h
)

target_include_directories(mongoose PUBLIC
                            ${CMAKE_CURRENT_SOURCE_DIR}
)