cmake_minimum_required(VERSION 2.8)

#---------- lp benchmark ----------
add_custom_target(lpbench ALL DEPENDS gen lpbdir)

add_custom_command(
    TARGET lpbench
    COMMAND for i in {1..10}\; do python gen_lp.py 8 lpbench/lpb_0008_\$\$i\; done
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "generate 10 lps of size 8"
)

add_custom_command(
    TARGET lpbench
    COMMAND for i in {1..10}\; do python gen_lp.py 16 lpbench/lpb_0016_\$\$i\; done
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "generate 10 lps of size 16"
)

add_custom_command(
    TARGET lpbench
    COMMAND for i in {1..10}\; do python gen_lp.py 32 lpbench/lpb_0032_\$\$i\; done
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "generate 10 lps of size 32"
)

add_custom_command(
    TARGET lpbench
    COMMAND for i in {1..10}\; do python gen_lp.py 64 lpbench/lpb_0064_\$\$i\; done
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "generate 10 lps of size 64"
)

add_custom_command(
    TARGET lpbench
    COMMAND for i in {1..10}\; do python gen_lp.py 128 lpbench/lpb_0128_\$\$i\; done
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "generate 10 lps of size 128"
)

add_custom_command(
    TARGET lpbench
    COMMAND for i in {1..10}\; do python gen_lp.py 256 lpbench/lpb_0256_\$\$i\; done
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "generate 10 lps of size 256"
)

add_custom_command(
    TARGET lpbench
    COMMAND for i in {1..10}\; do python gen_lp.py 512 lpbench/lpb_0512_\$\$i\; done
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "generate 10 lps of size 512"
)

add_custom_command(
    TARGET lpbench
    COMMAND for i in {1..10}\; do python gen_lp.py 1024 lpbench/lpb_1024_\$\$i\; done
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "generate 10 lps of size 1024"
)

#---------- benchmark dir ----------

add_custom_target(lpbdir)
add_custom_command(
    TARGET lpbdir
    COMMAND ${CMAKE_COMMAND} -E make_directory lpbench
    COMMENT "creating lpbench dir"
)

#---------- gen target ----------

add_custom_target(gen DEPENDS gen_lp.py donjlp.py cplexlp.py cpplexlp.py)

#---------- generator scripts ----------

add_custom_command(
    OUTPUT gen_lp.py
    COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/gen_lp.py gen_lp.py
    COMMENT "copy lp_gen.py"
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/gen_lp.py
)
#---------- template modules ----------

add_custom_command(
    OUTPUT cplexlp.py
    COMMAND cheetah compile cplexlp.tmpl
    COMMENT "cheetah compile cplexlp.tmpl --> cplexlp.py"
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/cplexlp.tmpl
)

add_custom_command(
    OUTPUT cpplexlp.py
    COMMAND cheetah compile cpplexlp.tmpl
    COMMENT "cheetah compile cpplexlp.tmpl --> cpplexlp.py"
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/cpplexlp.tmpl
)

add_custom_command(
    OUTPUT donjlp.py
    COMMAND cheetah compile donjlp.tmpl
    COMMENT "cheetah compile donjlp.tmpl --> donjlp.py"
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/donjlp.tmpl
)

#---------- templates ----------

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/cplexlp.tmpl
    COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/cplexlp.tmpl cplexlp.tmpl
    COMMENT "copy cplexlp.tmpl"
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/cplexlp.tmpl
)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/cpplexlp.tmpl
    COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/cpplexlp.tmpl cpplexlp.tmpl
    COMMENT "copy cpplexlp.tmpl"
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/cpplexlp.tmpl
)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/donjlp.tmpl
    COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/donjlp.tmpl donjlp.tmpl
    COMMENT "copy donjlp.tmpl"
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/donjlp.tmpl
)

set(cleanfiles "${cleanfiles};cplexlp.pyc")
set(cleanfiles "${cleanfiles};donjlp.pyc")
set_directory_properties(PROPERTY ADDITIONAL_MAKE_CLEAN_FILES "${cleanfiles}")
