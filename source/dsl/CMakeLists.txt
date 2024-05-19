# -------------------------------
# Macro that will define our source groups
# -------------------------------
MACRO(GROUPSOURCES curdir folder_name)
    FILE(GLOB children RELATIVE ${curdir} ${curdir}/*)
    FOREACH(child ${children})
           IF(IS_DIRECTORY ${curdir}/${child})
              GROUPSOURCES(${curdir}/${child} ${folder_name}/${child})
           ELSE()
              SOURCE_GROUP(${folder_name} FILES ${curdir}/${child})
           ENDIF()
    ENDFOREACH()
ENDMACRO(GROUPSOURCES)

if(DSL_STATIC_LIB)
    add_library(dsl STATIC)
else()
    add_library(dsl SHARED)
endif()

# Sources
## private sources
target_sources(dsl PRIVATE 
    ${SOURCE_DSL_DIRECTORY}/private/util/blob.cpp
    ${SOURCE_DSL_DIRECTORY}/private/util/blob_view.cpp
    ${SOURCE_DSL_DIRECTORY}/private/util/blob_reader.cpp
    ${SOURCE_DSL_DIRECTORY}/private/util/blob_writer.cpp
    ${SOURCE_DSL_DIRECTORY}/private/util/pointer_math.cpp
    ${SOURCE_DSL_DIRECTORY}/private/util/string_op.cpp)

## public sources
target_sources(dsl PUBLIC 
    ### fmt
    ${SOURCE_DSL_DIRECTORY}/public/fmt/core.h
    ${SOURCE_DSL_DIRECTORY}/public/fmt/format-inl.h
    ${SOURCE_DSL_DIRECTORY}/public/fmt/format.h

    ${SOURCE_DSL_DIRECTORY}/public/util/hash.h
    ${SOURCE_DSL_DIRECTORY}/public/util/string_op.h
    ${SOURCE_DSL_DIRECTORY}/public/util/defines.h
    ${SOURCE_DSL_DIRECTORY}/public/util/blob.h
    ${SOURCE_DSL_DIRECTORY}/public/util/blob_view.h
    ${SOURCE_DSL_DIRECTORY}/public/util/blob_reader.h
    ${SOURCE_DSL_DIRECTORY}/public/util/blob_writer.h
    ${SOURCE_DSL_DIRECTORY}/public/util/pointer_math.h
    ${SOURCE_DSL_DIRECTORY}/public/util/color.h
    ${SOURCE_DSL_DIRECTORY}/public/util/memory_size.h
    ${SOURCE_DSL_DIRECTORY}/public/util/log.h
    ${SOURCE_DSL_DIRECTORY}/public/util/assert.h
    ${SOURCE_DSL_DIRECTORY}/public/util/types.h)

# Create the project filters
GROUPSOURCES(${SOURCE_DSL_DIRECTORY}/private private)
GROUPSOURCES(${SOURCE_DSL_DIRECTORY}/public public)

# Private includes
target_include_directories(dsl PRIVATE ${SOURCE_DSL_DIRECTORY}/private)

# Public includes
target_include_directories(dsl PUBLIC ${SOURCE_DSL_DIRECTORY}/public)

# Compile definitions
if(WIN32)
    target_compile_definitions(dsl PUBLIC DSL_PLATFORM_WINDOWS)
endif()
                      