cmake_minimum_required(VERSION 3.12)

project(histogram LANGUAGES CXX VERSION 0.1)

# Boost histogram requires C++14
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Adding folders to keep the structure a bit nicer in IDE's
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# This is completely optional and just adds hints to IDEs - no affect on build at all.
file(GLOB_RECURSE BOOST_HIST_FILES "extern/histogram/include/*.hpp")

add_library(boost-histogram INTERFACE)

# These are the Boost header-only libraries required by Boost::Histogram
target_include_directories(boost-histogram INTERFACE
    extern/assert/include
    extern/callable_traits/include
    extern/config/include
    extern/container_hash/include
    extern/core/include
    extern/detail/include
    extern/histogram/include
    extern/integer/include
    extern/iterator/include
    extern/move/include
    extern/mp11/include
    extern/mpl/include
    extern/preprocessor/include
    extern/static_assert/include
    extern/throw_exception/include
    extern/type_index/include
    extern/type_traits/include
    extern/utility/include
    extern/variant/include
    )

# This makes IDE's like XCode mimic the Boost Histogram structure
source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR}/extern/histogram/include PREFIX "Header Files" FILES ${BOOST_HIST_FILES})

# Tests (Require pytest to be available)
include(CTest)

if(BUILD_TESTING)

    macro(ADD_BH_TEST TNAME)
        add_executable(${TNAME} extern/histogram/test/${TNAME}.cpp)
        target_link_libraries(${TNAME} PRIVATE boost-histogram)
        add_test(NAME ${TNAME} COMMAND ${TNAME})
    endmacro()

    ADD_BH_TEST(algorithm_project_test)
    ADD_BH_TEST(algorithm_reduce_test)
    ADD_BH_TEST(algorithm_sum_test)
    ADD_BH_TEST(axis_category_test)
    ADD_BH_TEST(axis_integer_test)
    ADD_BH_TEST(axis_option_test)
    ADD_BH_TEST(axis_regular_test)
    ADD_BH_TEST(axis_size)
    ADD_BH_TEST(axis_traits_test)
    ADD_BH_TEST(axis_variable_test)
    ADD_BH_TEST(axis_variant_test)
    # These require extra Boost libraries
    #    ADD_BH_TEST(boost_accumulators_support_test)
    #    ADD_BH_TEST(boost_range_support_test)
    #    ADD_BH_TEST(boost_units_support_test)
    ADD_BH_TEST(detail_test)
    ADD_BH_TEST(histogram_dynamic_test)
    ADD_BH_TEST(histogram_growing_test)
    ADD_BH_TEST(histogram_mixed_test)
    ADD_BH_TEST(histogram_operators_test)
    ADD_BH_TEST(histogram_test)
    ADD_BH_TEST(indexed_test)
    ADD_BH_TEST(internal_accumulators_test)
    ADD_BH_TEST(linearize_test)
    ADD_BH_TEST(meta_test)
    ADD_BH_TEST(storage_adaptor_test)
    ADD_BH_TEST(unlimited_storage_test)
    ADD_BH_TEST(utility_test)

endif()



