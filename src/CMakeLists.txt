cmake_minimum_required(VERSION 3.10)

project( helloworld_cli_AND_test_version VERSION ${PROJECT_VERSION})

# исполняемые файлы собираются из следующих файлов
add_executable( helloworld_cli main.cpp )
add_executable( test_version test_version.cpp )

# исполняемые файлы использу библиотеку lib
target_link_libraries( helloworld_cli lib )
target_link_libraries( test_version   lib )
# директория заголовка библиотеки
target_include_directories(
    helloworld_cli 
    PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/../lib
)
target_include_directories(
    test_version
    PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/../lib
)
# директория бинарника библиотеки
target_include_directories(
	helloworld_cli
	PRIVATE
	${CMAKE_CURRENT_BINARY_DIR}
)
target_include_directories(
	test_version
	PRIVATE
	${CMAKE_CURRENT_BINARY_DIR}
)


# один из исполняемых файлов использует библиотеки BOOST
find_package(Boost COMPONENTS unit_test_framework REQUIRED)
target_link_libraries( test_version ${Boost_LIBRARIES} )
# директория заголовков библиотеки
target_include_directories(
    test_version
    PRIVATE
    ${Boost_INCLUDE_DIR}
)
set_target_properties(
	test_version PROPERTIES
    COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
)

# поддержка станадарта языка
set_target_properties(
    helloworld_cli PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)
set_target_properties(
    test_version PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)


#
install(TARGETS helloworld_cli RUNTIME DESTINATION bin)
 
#
set(CPACK_GENERATOR DEB)
set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
set(CPACK_PACKAGE_CONTACT example@example.com)
include(CPack)

add_test(test_version test_version)
