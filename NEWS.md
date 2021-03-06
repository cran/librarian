# librarian 1.8.1 (2021-07-12)

- FIX - There was an issue where brand new GitHub packages wouldn't be installed unless `update_all = TRUE`. Closes #27.
- FIX - Changed link to CONDUCT.md in README to an absolute link for CRAN resubmission.
- FIX - Removed `LazyData` from DESCRIPTION for CRAN resubmission.
- FIX - Cached data is no longer used when checking if a package is installed.


# librarian 1.8.0 (2021-06-29)

- ADD - `stock()` lets you install packages from a list without attaching them, unlike `shelf()` which does both. Closes #26.
- MOD - `shelf()` was refactored to pull package installation into the new `stock()` function.
- FIX - An internal QA test was failing because the dependency list of the package `testthat` has changed since the last `librarian` version c. 2019. Updated the list.
- FIX - Results of `stock()` and `shelf()` are now reported in the same order as the user gave them in `...`.
- MOD - `update_all = TRUE` in `shelf()` and `stock()` now force reinstallation of a GitHub package even if the remote package's SHA1 matches the installed version. The reasoning is a) that 'forcing an update' should actually perform an update even if the files are identical, and b) in the event that some of the user's files have been changed, the stored SHA1 will not reflect this. Closes #24.
- FIX - The "paths '-1' are not writeable" error was apparently fixed in v1.7.2, but that version was never uploaded to CRAN. Uploading it to CRAN should hopefully solve this once and for all. Closes #21. Closes #23.



# librarian 1.7.2 (2019-03-11)

- FIX - `lib_paths()` doesn't call itself as an argument in `shelf()`. Thanks, [Miles Smith](https://github.com/milescsmith/)!
- FIX - `shelf()` now exits as soon as possible by trying to attach all packages right at the start of the function. Previously, package attachment was done only at the end, after all the other checks were done. If you set `update_all = TRUE` then the function will run package installation first and package attachment second. Closes #19.
- FIX - `unshelf()` only acts on dependencies that are actually attached. It previously also acted on packages that were only namespaced but not attached. Closes #20.
- FIX - `make_dirs()` no longer validates filenames that contain dots as if they were filenames, e.g. `C:/MyPath/R/3.5`.
- FIX - `lib_paths()` shows names of non-writeable paths, instead of the result `-1`.



# librarian 1.7.0 (2019-03-13)

- FIX - You can now detach everything except R's default packages by using `safe = FALSE` in `unshelf()`. If `safe = TRUE`, the packages that you've set as default ones in your .RProfile (via `lib_startup()` for example) will not be detached. Fixes #17.
- ADD - `check_attached()` and `check_installed()`, which lets you check the status of packages. If you run the functions with no packages named, they will return a list of all packages that are attached and installed.
- MOD - `shelf()`, `unshelf()`, `reshelf()`, `check_attached()`, and `check_installed()` can now accept four different input methods:
   1. A comma-separated list of bare names. `shelf(package, name, here)`
   2. A comma-separated list of strings. `shelf("package", "name", "here")`
   3. A comma-separated list of names and strings. `shelf("package", name, "here")`
   4. A character vector of package names, as long as it is the only item passed in `...`. `shelf(c("package", "name", "here"))`
   - These changes were made to accommodate the advanced input methods of `remotes`. They also make it possible to use these functions inside scripts, like passing package lists around.
- ADD - Many new unit tests, with more stringent testing methods.
- MOD - Major refactoring. The biggest change is that user-facing stuff like messages have been extracted from the functions so that the logic of the function is easier to maintain.



# librarian 1.6.1 (2019-03-09)

- FIX - `unshelf()` with `everything = TRUE` arg now detaches **all** non-default packages if `safe = FALSE` is also set. In the past, it would leave behind the packages that you had added to your .Rprofile using `lib_startup()`. This is still the default behaviour.
- ADD - Various behind-the-scenes helper functions in preparation for a code refactor.



# librarian 1.6.0 (2019-02-19)

- MOD - `biocLite` has been replaced with `BiocManager` for installing BioConductor packages. `BiocManager` has been added as a dependency.
- FIX - Various spelling errors in preparation for CRAN submission.
- ADD - Vignette for CRAN
- ADD - `browse_cran()` for searching CRAN package titles and descriptions by keyword or regular expression.



# librarian 1.5.5 (2019-02-18)

- FIX - `nse_dots()` used to insert spaces in names that included `-` because it was formatting them nicely as if they were expressions with the minus operator. The function now removes any spaces that were created in the process of converting from a symbol to a string. Closes [#13](https://github.com/DesiQuintans/librarian/issues/13).
- MOD - `remotes` replaces `devtools`. Closes [#11](https://github.com/DesiQuintans/librarian/issues/11).
- REM - The `backports` package is no longer needed.


# librarian 1.5.4 (2019-02-15)

- FIX - The regular expression used in `librarian:::check_installed()` failed if the package name had a `.` inside it, so those packages were installable but not attachable with _librarian_. Closes [#12](https://github.com/DesiQuintans/librarian/issues/12).



# librarian 1.5.3 (2019-01-24)

- MOD - Less alarming and more informative message (not a warning) for when the `cran_repo` argument in `shelf()` contains the default R value `@CRAN@`. Closes [Issue #10](https://github.com/DesiQuintans/librarian/issues/10). Thanks, Jim Hunter!


# librarian 1.5.2 (2018-11-19)

- FIX - The code comment that `lib_startup()` uses to mark the lines it edited previously indicated an old function name.

# librarian 1.5.1 (2018-11-15)

- MOD - `check_installed()` now works much faster, so librarian in general should work much faster because `check_installed()` was called in every function.

# librarian 1.5.0 (2018-11-02)

- ADD - `lib_startup()` lets you set library search paths and packages that will be loaded globally at the start of every R session, or on a per-project basis.


# librarian 1.4.0

- ADD - `shelf()` now supports installing Bioconductor packages via `biocLite()`. Bioconductor's `Biobase` package needs to be installed before *librarian* will attempt to install Bioconductor packages.

# librarian 1.3.2

- REVERT - R dependency back to => 3.4.0.

# librarian 1.3.1

- FIX - librarian was written for R 3.5.0 but only asked for 3.4.0 as a dependency. The R dependency is now 3.5.0.
- MOD - I have added a dependency for `backports` so that users of R < 3.5.0 do not have to update their R installation for only one important function that librarian needs (`...length()`). Closes issue #4.
- FIX - Sometimes the CRAN URL check would return a vector of length >1, raising a warning message.

# librarian 1.3.0

- ADD - `lib_paths()` is a wrapper for `.libPaths()` with folder creation built-in. It
  lets you name and create folders where new packages will be installed, view the folders
  that are on the package search path, and reshuffle their order.
- ADD - `lib` argument for `shelf()` lets you specify the folder where new packages will
  be installed. The `ask` argument controls whether R asks for permission before creating
  this folder; set `ask = FALSE` to create the folder automatically.
    - If the packages are already installed, they will be loaded from their current
      location and not re-installed.
    - If you set `update_all = TRUE`, a new copy of the package will be installed to
      `lib`. This means that you can potentially have several copies of the same package
      across many folders on your machine, each a different version. I felt that this was
      reasonable so that you could maintain a different library folder for different
      projects, and updates that you made in Project B would not affect the package
      versions you rely on for Project A.
- FIX - The `cran_repo` warning raised by `shelf()` now shows the original string.
- FIX - Unexported functions are now properly documented.
- FIX - `unshelf()` raises an error when you haven't told it to detach anything.

# librarian 1.2.0

- FIX - `shelf()` now sets a default CRAN repo properly on the command line.
    - REM - The `custom_repo` argument in `shelf()` has been renamed to `cran_repo`.
    - `cran_repo` arg checks that its value is a valid URL. I previously supported
      `custom_repo = NULL` because the base `install.packages()` uses `NULL` to signal
      installation from a local file, but the point of _librarian_ is to install CRAN and
      GitHub packages from the net, so `cran_repo` does not keep this functionality.
- MOD - Improved the documentation following CRAN feedback.
- ADD - `shelf()`, `unshelf()`, and `reshelf()` now invisibly return named vectors
  describing the packages that were operated on and whether they were successfully
  attached or detached.
- ADD - Unit tests to make sure that my fixes don't break stuff.
- ADD - `unshelf(everything = TRUE)` argument detaches all packages except for the default
  ones.
- ADD - `unshelf(safe = TRUE)` argument checks if packages are still needed by others
  before detaching them.
- ADD - `unshelf(warn = TRUE)` argument will print a Message if packages were not detached
  (because `safe = TRUE` and the packages were still needed).
- ADD - `unshelf(..., also_depends = TRUE)` argument detaches packages named in `...` as
  well as their dependencies.
    - With the `safe` and `quiet` arguments defaulting to `TRUE`, the default behaviour is
      to leave packages behind if other packages in the search path still need them, but
      not to interrupt the user with a message about it. `unshelf()` still invisibly
      returns the success/failure for each package it attempted to detached.
    - Looking through the search path is pretty slow, I don't recommend it for sessions
      with lots of packages!
- MOD - The new dependency-checking code needs the `tools` package, but it's distributed
  with R.

# librarian 1.1.0

- ADD - `reshelf()` for refreshing a package. Useful for loading new builds of your
  personal package.

# librarian 1.0.3

- ADD - `unshelf()` can handle the Github Username/packagename format now, instead of
  requiring the user to provide only the package name. The biggest effect of this change
  is that if you want to unload your packages, you can now just change your `shelf()` to
  `unshelf()` and run it.
- ADD - `shelf()` and `unshelf()` check for duplicates in the package list you provide.

# librarian 1.0.2

- FIX - Many documentation changes for CRAN submission.
- FIX - Import `utils` (a default package) for base R's package-handling functions.
  Omitting this caused warnings in R CMD CHECK.
- FIX - Bug in `unshelf()` that made it try to unload packages even if they were not
  loaded.
- ADD - `custom_repo` argument for `shelf()`, which defaults to R's default behaviour.

# librarian 1.0.1

- REM - No longer imports `rlang`. Only imports `devtools` now.
- ADD - `shelf()` now returns `devtools::session_info()` invisibly so that you can print
  it.
- ADD - `NEWS.md` file to track changes to the package.
- ADD - info re. updating package dependencies.

# librarian 1.0.0

- Initial release. Includes `shelf()` and `unshelf()` in feature-complete form.
