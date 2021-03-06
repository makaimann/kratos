add_library(kratos port.cc port.hh generator.cc generator.hh
        expr.hh context.hh expr.cc context.cc
        codegen.cc codegen.hh stmt.cc stmt.hh pass.cc pass.hh
        ir.cc ir.hh graph.cc graph.hh hash.cc hash.hh util.cc util.hh except.cc except.hh fsm.cc fsm.hh
        syntax.hh syntax.cc tb.hh tb.cc debug.hh debug.cc db.hh sim.cc sim.hh eval.cc eval.hh interface.cc interface.hh
        lib.cc lib.hh fault.cc fault.hh formal.cc formal.hh)

target_include_directories(kratos PUBLIC ../extern/fmt/include ../extern/cxxpool/src ../extern/sqlite_orm/include
        ../extern/sqlite/include)
if (WIN32 OR MINGW OR APPLE)
   target_link_libraries(kratos fmt sqlite3)
else()
    if (${INCLUDE_FILESYSTEM})
        target_link_libraries(kratos fmt sqlite3 stdc++fs)
    else()
        target_link_libraries(kratos fmt sqlite3)
    endif()
endif()

if (APPLE)
    set_target_properties(kratos PROPERTIES CXX_VISIBILITY_PRESET hidden)
    set_target_properties(fmt PROPERTIES CXX_VISIBILITY_PRESET hidden)
endif()

# turn all the warnings on
if (MSVC)
  target_compile_options(kratos PRIVATE /W4 /WX)
else()
  target_compile_options(kratos PRIVATE -Wall -Wextra -pedantic -Werror)
endif()

# clang-tidy
if (${USE_CLANG_TIDY})
    MESSAGE("Running clang-tidy")
    set_target_properties(kratos PROPERTIES CXX_CLANG_TIDY "clang-tidy")
endif()
