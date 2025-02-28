if (NOT WITH_SYSTEM_PROVIDED_3PARTY)
  # Configure expat library.
  # Suppress "Policy CMP0077 is not set: option() honors normal variables"
  # for the expat options below.
  set(CMAKE_POLICY_DEFAULT_CMP0077 NEW)
  set(EXPAT_BUILD_TOOLS OFF)
  set(EXPAT_BUILD_EXAMPLES OFF)
  set(EXPAT_BUILD_TESTS OFF)
  set(EXPAT_BUILD_DOCS OFF)
  set(EXPAT_BUILD_PKGCONFIG OFF)
  set(EXPAT_ENABLE_INSTALL OFF)
  set(EXPAT_SHARED_LIBS OFF)
  add_subdirectory(expat/expat)

  # Configure Jansson library.
  set(JANSSON_BUILD_DOCS OFF)
  set(JANSSON_BUILD_MAN OFF)
  set(JANSSON_EXAMPLES OFF)
  set(JANSSON_INSTALL OFF)
  set(JANSSON_WITHOUT_TESTS ON)
  add_subdirectory(jansson/jansson/)
  target_include_directories(jansson INTERFACE "${PROJECT_BINARY_DIR}/3party/jansson/jansson/include")

  # Add gflags library.
  add_subdirectory(gflags)
  target_compile_options(gflags_nothreads_static PRIVATE $<$<CXX_COMPILER_ID:GNU>:-Wno-subobject-linkage>)
  # Not needed for the usual build process, but it fixes QtCreator editor,
  # that doesn't see gflags/gflags.h in binary dir (gflags has tricky cmake configuration).
  if (PLATFORM_DESKTOP)
    include_directories("${PROJECT_BINARY_DIR}/3party/gflags/include")
  endif()

  # Add pugixml library.
  add_subdirectory(pugixml)

  # Add protobuf library.
  add_subdirectory(protobuf)
endif()

add_subdirectory(agg)
add_subdirectory(bsdiff-courgette)

if (NOT LINUX_DETECTED)
  add_subdirectory(freetype)
  add_subdirectory(icu)
endif()

add_subdirectory(liboauthcpp)
add_subdirectory(minizip)
add_subdirectory(opening_hours)
add_subdirectory(sdf_image)
add_subdirectory(stb_image)
add_subdirectory(succinct)
add_subdirectory(open-location-code)
add_subdirectory(vulkan_wrapper)

if (PLATFORM_DESKTOP)
  add_subdirectory(libtess2)
endif()
