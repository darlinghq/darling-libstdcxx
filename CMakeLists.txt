project(darling-libstdcxx)

add_compile_options(
	-nostdinc
	-nostdinc++
	-w
	-std=c++14
)

include_directories(BEFORE
	${CMAKE_CURRENT_SOURCE_DIR}/include/c++/4.2.1
	${CMAKE_CURRENT_SOURCE_DIR}/include/c++/4.2.1/backward
)

add_definitions(
	-DBUILDING_LIBSTDCXX
	-D_LIBCPPABI_VERSION=1
)

set(libstdcxx_sources
	src/allocator-inst.cc
	src/atomicity.cc
	src/basic_file.cc
	src/bitmap_allocator.cc
	src/c++locale.cc
	src/codecvt.cc
	src/codecvt_members.cc
	src/collate_members.cc
	src/compatibility.cc
	src/compatibility-ldbl.cc
	src/complex_io.cc
	src/concept-inst.cc
	src/ctype.cc
	src/ctype_members.cc
	src/debug.cc
	src/debug_list.cc
	src/del_op.cc
	src/del_opnt.cc
	src/del_opv.cc
	src/del_opvnt.cc
	src/eh_exception.cc
	src/eh_terminate.cc
	src/ext-inst.cc
	src/fstream-inst.cc
	src/functexcept.cc
	src/globals_io.cc
	src/ios.cc
	src/ios_failure.cc
	src/ios_init.cc
	src/ios_locale.cc
	src/ios-inst.cc
	src/iostream-inst.cc
	src/istream.cc
	src/istream-inst.cc
	src/limits.cc
	src/list.cc
	src/locale.cc
	src/locale_facets.cc
	src/locale_init.cc
	src/locale-inst.cc
	src/localename.cc
	src/messages_members.cc
	src/misc-inst.cc
	src/monetary_members.cc
	src/mt_allocator.cc
	src/new_handler.cc
	src/new_op.cc
	src/new_opnt.cc
	src/new_opv.cc
	src/new_opvnt.cc
	src/numeric_members.cc
	src/ostream-inst.cc
	src/pool_allocator.cc
	src/sstream-inst.cc
	src/stdexcept.cc
	src/streambuf.cc
	src/streambuf-inst.cc
	src/string-inst.cc
	src/strstream.cc
	src/time_members.cc
	src/tinfo.cc
	src/tree.cc
	src/valarray-inst.cc
	src/wlocale-inst.cc
	src/wstring-inst.cc
)

set_property(SOURCE src/concept-inst.cc APPEND_STRING
	PROPERTY COMPILE_DEFINITIONS
		"_GLIBCXX_CONCEPT_CHECKS"
)

set(DYLIB_INSTALL_NAME "/usr/lib/libstdc++.6.dylib")
set(DYLIB_COMPAT_VERSION "7.0.0")
set(DYLIB_CURRENT_VERSION "104.1.0")

add_darling_library(stdcxx SHARED ${libstdcxx_sources})
make_fat(stdcxx)
target_link_libraries(stdcxx PRIVATE system cxxabi)
set_target_properties(stdcxx PROPERTIES
	OUTPUT_NAME "stdc++.6"
)
set_property(TARGET stdcxx APPEND_STRING
	PROPERTY LINK_FLAGS 
		" -Wl,-force_symbols_not_weak_list,${CMAKE_CURRENT_SOURCE_DIR}/exports/notweak.exp -Wl,-force_symbols_weak_list,${CMAKE_CURRENT_SOURCE_DIR}/exports/weak.exp -Wl,-reexport_library,${CMAKE_BINARY_DIR}/src/external/libcxxabi/libc++abi.dylib "
)
add_dependencies(stdcxx cxxabi)

install(TARGETS stdcxx DESTINATION libexec/darling/usr/lib)

