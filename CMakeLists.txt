cmake_minimum_required(VERSION 3.25)
project(
    EmbeddedSWBlog
    VERSION 1.0.0.0
    LANGUAGES NONE
)

find_program(
    SPHINX_EXECUTABLE
    NAMES sphinx-build sphinx-build.exe
)

add_custom_target(posts ALL
	${SPHINX_EXECUTABLE} -M html ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR}/blog
)
