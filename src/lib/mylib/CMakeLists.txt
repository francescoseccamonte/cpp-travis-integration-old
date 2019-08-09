add_library(mylib
            mylib.cpp)

target_include_directories(mylib PUBLIC include)

install(DIRECTORY include/mylib DESTINATION include)
install(TARGETS mylib ${DEFAULT_DESTINATIONS})

if(BUILD_TESTING)
	add_subdirectory(test)
endif()
