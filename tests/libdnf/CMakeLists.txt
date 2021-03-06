ADD_EXECUTABLE(test_libdnf_main dnf-self-test.c)



TARGET_LINK_LIBRARIES(test_libdnf_main
                      libdnf
                      ${REPO_LIBRARIES}
                      ${GLIB_LIBRARIES}
                      ${GLIB_GOBJECT_LIBRARIES}
                      ${GLIB_GIO_LIBRARIES}
                      ${GLIB_GIO_UNIX_LIBRARIES}
                      ${SOLV_LIBRARY}
                      ${SOLVEXT_LIBRARY}
                      ${RPMDB_LIBRARY})

ADD_TEST(test_libdnf_main test_libdnf_main)
IF (NOT DISABLE_VALGRIND AND VALGRIND_PROGRAM)
    ADD_TEST(test_libdnf_valgrind ${VALGRIND_PROGRAM} --error-exitcode=1 --leak-check=full
             --suppressions=${CMAKE_SOURCE_DIR}/tests/glib.supp
             --suppressions=${CMAKE_SOURCE_DIR}/tests/nss.supp
             --suppressions=${CMAKE_SOURCE_DIR}/tests/giovfs.supp
             --suppressions=${CMAKE_SOURCE_DIR}/tests/librepo.supp
             ${CMAKE_CURRENT_BINARY_DIR}/test_libdnf_main)
ENDIF()

IF (WITH_SWDB)
    ADD_EXECUTABLE(test_libdnf_swdb dnf-swdb-test.c)

    TARGET_LINK_LIBRARIES(test_libdnf_swdb
        libdnf
        ${REPO_LIBRARIES}
        ${GLIB_LIBRARIES}
        ${GLIB_GOBJECT_LIBRARIES}
        ${GLIB_GIO_LIBRARIES}
        ${GLIB_GIO_UNIX_LIBRARIES}
        ${SOLV_LIBRARY}
        ${SOLVEXT_LIBRARY}
        ${RPMDB_LIBRARY}
        ${SQLite3_LIBRARIES})

    ADD_TEST(test_libdnf_swdb test_libdnf_swdb)
    IF (NOT DISABLE_VALGRIND AND VALGRIND_PROGRAM)
        ADD_TEST(test_libdnf_swdb_valgrind ${VALGRIND_PROGRAM}
                 --error-exitcode=1
                 ${CMAKE_CURRENT_BINARY_DIR}/test_libdnf_swdb)
    ENDIF()

ENDIF()
