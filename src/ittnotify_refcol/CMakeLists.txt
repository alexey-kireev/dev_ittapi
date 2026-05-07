add_library(ittnotify_refcol SHARED itt_refcol_impl.c)

target_include_directories(ittnotify_refcol
    PUBLIC $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    PRIVATE ${CMAKE_SOURCE_DIR}/src/ittnotify
)

target_link_libraries(ittnotify_refcol PRIVATE ${CMAKE_DL_LIBS})
set_target_properties(ittnotify_refcol PROPERTIES
    LINKER_LANGUAGE C
    RUNTIME_OUTPUT_DIRECTORY ${LIBRARY_OUTPUT_PATH}
    ARCHIVE_OUTPUT_DIRECTORY ${LIBRARY_OUTPUT_PATH})

if(WIN32)
    set_target_properties(ittnotify_refcol PROPERTIES
        OUTPUT_NAME libittnotify_refcol
        PREFIX ""
        WINDOWS_EXPORT_ALL_SYMBOLS ON)
else()
    set_target_properties(ittnotify_refcol PROPERTIES OUTPUT_NAME ittnotify_refcol)
endif()

option(ITT_API_REFCOL_SMOKE_TESTS "Build reference collector smoke tests" OFF)
if(ITT_API_REFCOL_SMOKE_TESTS)
    add_executable(refcol_smoke_test tests/smoke_test.c)
    target_include_directories(refcol_smoke_test
        PRIVATE ${CMAKE_SOURCE_DIR}/include
                ${CMAKE_SOURCE_DIR}/src/ittnotify
    )
    target_link_libraries(refcol_smoke_test PRIVATE ittnotify ${CMAKE_DL_LIBS})
    set_target_properties(refcol_smoke_test PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY ${LIBRARY_OUTPUT_PATH})
endif()
