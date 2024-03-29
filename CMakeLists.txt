CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

include(RezBuild)
include(RezRepository)
include(ExternalProject)

# CMake
rez_set_archive(
    CMAKE_URL cmake/cmake-${REZ_BUILD_PROJECT_VERSION}-linux-x86_64.tar.gz
    https://github.com/Kitware/CMake/releases/download/v${REZ_BUILD_PROJECT_VERSION}/cmake-${REZ_BUILD_PROJECT_VERSION}-linux-x86_64.tar.gz
)

# The environment variable REZ_TMP_PATH is meant to point to a location such as "/tmp/rez".
set(EXTRACT_DIR $ENV{REZ_TMP_PATH}/cmake/${REZ_BUILD_PROJECT_VERSION})

# We configure/build/install CMake through the ExternalProject_Add macro,
# using three different shell scripts for each of these steps.
# If necessary, it is possible to skip one of these steps by replacing the command
# by "", an empty string.
ExternalProject_Add(
    REZ_CMAKE
    URL ${CMAKE_URL}
    PREFIX cmake
    SOURCE_DIR ${EXTRACT_DIR}
    PATCH_COMMAND ""
    UPDATE_COMMAND ""
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ""
    INSTALL_COMMAND bash ${CMAKE_SOURCE_DIR}/install.sh ${EXTRACT_DIR}
)

# We use a bogus CMake "install" step in order to be able to properly finish
# and get out of the CMake script.
install(
    CODE
    "execute_process(
        COMMAND echo \"\n[CMAKE] CMake-${REZ_BUILD_PROJECT_VERSION} PACKAGE SETUP FINISHED!\n\"
    )"
)
