set(APP_SRC 
	${CMAKE_CURRENT_SOURCE_DIR}/../src/sch_api.c
	${CMAKE_CURRENT_SOURCE_DIR}/../src/sch_app.c 
	${CMAKE_CURRENT_SOURCE_DIR}/../src/sch_cmds.c)
	
set(APP_INC_DIRS 
	${CMAKE_CURRENT_SOURCE_DIR}/../src/)

include_directories(${APP_INC_DIRS})
include_directories(${CFE_INC_DIRS})
include_directories(${OSAL_INC_DIRS})
include_directories(${PSP_INC_DIRS})
	
add_library(${APP_FILE} SHARED ${APP_SRC} ${APP_CUSTOM_SRC})

add_library(sch_def_msgtbl STATIC ${TABLE_DIR}/sch_def_msgtbl.c)
set_target_properties(sch_def_msgtbl PROPERTIES PREFIX "" SUFFIX ".tbl")

add_library(sch_def_schtbl STATIC ${TABLE_DIR}/sch_def_schtbl.c)
set_target_properties(sch_def_schtbl PROPERTIES PREFIX "" SUFFIX ".tbl")