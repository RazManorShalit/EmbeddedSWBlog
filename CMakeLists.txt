cmake_minimum_required(VERSION 3.25)
project(
    EmbeddedSWBlog
    VERSION 1.0.0.0
    LANGUAGES NONE
)

set(Python_FIND_VIRTUALENV ONLY)
find_package(Python COMPONENTS Interpreter REQUIRED)

get_filename_component(PYTHON_BIN_DIR "${Python_EXECUTABLE}" DIRECTORY)

find_program(SPHINX_EXECUTABLE
    NAMES sphinx-build
    HINTS "${PYTHON_BIN_DIR}"
    NO_DEFAULT_PATH
)

add_custom_target(posts ALL
	${SPHINX_EXECUTABLE} -b html -W ${CMAKE_CURRENT_SOURCE_DIR}/sources ${CMAKE_CURRENT_BINARY_DIR}/blog/html -d ${CMAKE_CURRENT_BINARY_DIR}/blog/doctrees
    COMMENT "Building blog posts with Sphinx"
)
