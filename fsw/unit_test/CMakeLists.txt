include(CTest)
find_program( MEMORYCHECK_COMMAND valgrind )
find_package(Threads)
set( MEMORYCHECK_COMMAND_OPTIONS "--trace-children=yes --leak-check=full" )

# Set include directories
include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}
	${CFE_INC_DIRS}
	${OSAL_INC_DIRS}
	${UTF_INCS}
	${APP_INC_DIRS})

add_definitions(-DCFE_LINUX)

set(CMAKE_C_FLAGS "-g -O0 -Wformat=0 -Wno-pointer-to-int-cast -Wno-int-to-pointer-cast -fprofile-arcs -ftest-coverage -m32")

# Add the executables
add_executable(SCH-UT 
		${CMAKE_CURRENT_SOURCE_DIR}/utf_test_sch.c
		${APP_SRC}
		${APP_CUSTOM_SRC}
		${UTF_SRC})
target_link_libraries(SCH-UT -fprofile-arcs gcov ${CMAKE_THREAD_LIBS_INIT})

# We need to both run the test and compare the as-run with the expected output
# with a diff command. 
add_test(NAME sch-ut
    COMMAND ${CMAKE_COMMAND}
    	-DTEST_PROG=$<TARGET_FILE:SCH-UT>
    	-DEXPECTED_OUTPUT=${UNIT_TEST_INPUT_DIR}/sch_utf_test.out
    	-DACTUAL_OUTPUT=${CMAKE_CURRENT_BINARY_DIR}/sch_utf_test.out
    -P ${CMAKE_CURRENT_SOURCE_DIR}/runtest.cmake)

# Copy input files to build directory
configure_file(${UNIT_TEST_INPUT_DIR}/sch_utf_cmds.in ${CMAKE_CURRENT_BINARY_DIR} COPYONLY)
configure_file(${UNIT_TEST_INPUT_DIR}/sch_utf_hk.in ${CMAKE_CURRENT_BINARY_DIR} COPYONLY)
configure_file(${UNIT_TEST_INPUT_DIR}/sch_utf_hk2.in ${CMAKE_CURRENT_BINARY_DIR} COPYONLY)
    
# Build tables for the tests
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/cf/apps)
SET(CMAKE_CXX_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}/cf/apps")
SET(CMAKE_C_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}/cf/apps")
add_library(ut_sch_def_schtbl  STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_schtbl.c)
add_library(ut_sch_def_schtbl1 STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_schtbl1.c)
add_library(ut_sch_def_schtbl2 STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_schtbl2.c)
add_library(ut_sch_def_schtbl3 STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_schtbl3.c)
add_library(ut_sch_def_schtbl4 STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_schtbl4.c)
add_library(ut_sch_def_schtbl5 STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_schtbl5.c)
add_library(ut_sch_def_schtbl6 STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_schtbl6.c)
add_library(ut_sch_def_msgtbl  STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_msgtbl.c)
add_library(ut_sch_def_msgtbl7 STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_msgtbl7.c)
add_library(ut_sch_def_msgtbl8 STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_msgtbl8.c)
add_library(ut_sch_def_msgtbl9 STATIC ${UNIT_TEST_INPUT_DIR}/cf/apps/sch_def_msgtbl9.c)
set_target_properties(ut_sch_def_schtbl PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_schtbl1 PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_schtbl2 PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_schtbl3 PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_schtbl4 PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_schtbl5 PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_schtbl6 PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_msgtbl  PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_msgtbl7 PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_msgtbl8 PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(ut_sch_def_msgtbl9 PROPERTIES PREFIX "" SUFFIX ".tbl")


