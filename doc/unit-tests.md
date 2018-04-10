Compiling/running unit tests
------------------------------------

Unit tests will be automatically compiled if dependencies were met in `./configure`
and tests weren't explicitly disabled.

After configuring, they can be run with `make check`.

To run the redend tests manually, launch `src/test/test_reden`.

To add more redend tests, add `BOOST_AUTO_TEST_CASE` functions to the existing
.cpp files in the `test/` directory or add new .cpp files that
implement new BOOST_AUTO_TEST_SUITE sections.

To run the reden-qt tests manually, launch `src/qt/test/test_reden-qt`

To add more reden-qt tests, add them to the `src/qt/test/` directory and
the `src/qt/test/test_main.cpp` file.
