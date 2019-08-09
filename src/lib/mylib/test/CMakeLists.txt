add_executable(mylib-tests mylib-tests.cpp)
target_link_libraries(mylib-tests CONAN_PKG::gtest mylib)

add_test(NAME mylib-tests
	COMMAND mylib-tests)
