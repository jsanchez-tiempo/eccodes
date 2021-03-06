# data/gts/CMakeLists.txt
# Download all the GTS data and reference files from website
#
file(READ "gts_data_files.txt" gts_files_to_download)
string(REGEX REPLACE "\n" ";" gts_files_to_download "${gts_files_to_download}")

file(READ "gts_ref_files.txt" gts_refs_to_download)
string(REGEX REPLACE "\n" ";" gts_refs_to_download "${gts_refs_to_download}")

if( ENABLE_EXTRA_TESTS )
    ecbuild_get_test_multidata(
        TARGET eccodes_download_gts
        NOCHECK
        NAMES ${gts_files_to_download} ${gts_refs_to_download}
    )
endif()

# Copy other files - e.g. text files etc from the source data/gts dir
list(APPEND other_files
    gts_data_files.txt
    gts_ref_files.txt
)

foreach( file ${other_files} )
   execute_process( COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/${file} ${CMAKE_CURRENT_BINARY_DIR} )
endforeach()
