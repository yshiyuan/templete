#--- 获取git commit信息 ---#
execute_process(COMMAND git log -1 --format=%H
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    OUTPUT_VARIABLE  GIT_REPO_HASH
    OUTPUT_STRIP_TRAILING_WHITESPACE
)
execute_process(COMMAND git log -1 --format=%an
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    OUTPUT_VARIABLE  GIT_REPO_AUTHOR
    OUTPUT_STRIP_TRAILING_WHITESPACE
)
execute_process(COMMAND git log -1 --format=%cd --date=format:%H:%M:%S@%Y/%m/%d
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    OUTPUT_VARIABLE  GIT_REPO_DATE
    OUTPUT_STRIP_TRAILING_WHITESPACE
)
set(_VERSION_CHECK "ver=${GIT_REPO_HASH}::author=${GIT_REPO_AUTHOR}::time=${GIT_REPO_DATE}")

#--- 使用__SHORT_FORM_FILE__代替_FILE_,避免文件路径过长 ---#
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D__SHORT_FORM_FILE__='\"$(subst ${SELF_SOURCE_DIR}/,,$(abspath $<))\"'") 

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D_DIR=\\\"${_DIR}\\\" -D_VERSION_CHECK=\\\"${_VERSION_CHECK}\\\"")
message("${CMAKE_CXX_FLAGS}")