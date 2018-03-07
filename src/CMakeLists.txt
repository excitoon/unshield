LIST(FIND CMAKE_PLATFORM_IMPLICIT_LINK_DIRECTORIES "${CMAKE_INSTALL_PREFIX}/lib" isSystemDir)
IF("${isSystemDir}" STREQUAL "-1")
SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}")
ENDIF("${isSystemDir}" STREQUAL "-1")

add_executable(unshield "unshield.c")
target_link_libraries(unshield libunshield)
if(HAVE_ICONV AND WIN32)
    target_link_libraries(unshield iconv)
endif()

add_executable(unshield-deobfuscate "unshield-deobfuscate.c")
target_link_libraries(unshield-deobfuscate libunshield)

if(BUILD_STATIC)
set_target_properties(unshield PROPERTIES LINK_FLAGS "-static -static-libgcc")
set_target_properties(unshield-deobfuscate PROPERTIES LINK_FLAGS "-static -static-libgcc")
endif()

install(TARGETS unshield RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR} LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})