#
# project: yojimbo
#

if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)

    # include the fips main cmake file
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")

    fips_setup(PROJECT yojimbo)
endif()

fips_include_directories(. netcode.io reliable.io)

# yojimbo
fips_begin_lib(yojimbo)
    add_definitions(
        -DNETCODE_ENABLE_TESTS=1
        -DRELIABLE_ENABLE_TESTS=1
        )
    fips_files(
        yojimbo.h yojimbo.cpp
        tlsf/tlsf.h tlsf/tlsf.c
        netcode.io/netcode.c netcode.io/netcode.h
        reliable.io/reliable.c reliable.io/reliable.h
    )
    fips_deps(mbedtls sodium)
fips_end_lib(yojimbo)

fips_begin_app(test cmdline)
    fips_files(test.cpp)
    fips_deps(yojimbo)
fips_end_app(test)

if (NOT FIPS_IMPORT)
fips_finish()
endif()