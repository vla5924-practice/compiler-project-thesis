cmake_minimum_required(VERSION 3.22)

project(CompilerPaper LANGUAGES NONE)

find_program(LATEXMK_COMMAND latexmk REQUIRED)

set(XELATEX_SOURCE_DIR "${CMAKE_SOURCE_DIR}/thesis")
set(XELATEX_OUTPUT_DIR "${CMAKE_BINARY_DIR}/docs")

add_custom_target(
    pdf ALL
    COMMAND ${LATEXMK_COMMAND} -f -xelatex -shell-escape -bibtex -outdir=${CMAKE_BINARY_DIR} "${XELATEX_SOURCE_DIR}/main.tex" 
    WORKING_DIRECTORY ${XELATEX_SOURCE_DIR}
    COMMENT "Render PDF with XeLaTeX"
)

add_custom_command(
    TARGET pdf POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E make_directory ${XELATEX_OUTPUT_DIR}
    COMMAND ${CMAKE_COMMAND} -E copy_if_different "${CMAKE_BINARY_DIR}/main.pdf" "${XELATEX_OUTPUT_DIR}/thesis.pdf"
    COMMENT "Copy PDF into ${XELATEX_OUTPUT_DIR}"
)
