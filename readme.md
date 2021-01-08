# Bazel rules for Qt [![ci status](https://circleci.com/gh/limdor/bazel_rules_qt.png)](https://app.circleci.com/pipelines/github/limdor/bazel_rules_qt?branch=master) [![Build status](https://ci.appveyor.com/api/projects/status/8y13hl0xuw37hchl/branch/master?svg=true)](https://ci.appveyor.com/project/limdor/bazel-rules-qt/branch/master)

These bazel rules and BUILD targets make it easy to use Qt from C++ projects built with bazel.

Note that unlike many libraries used through bazel, it requires Qt to be installed when building the application.
Also keep in mind that this rules only allow to link Qt dynamically.
In addition in the case of Linux it also requires Qt to be installed on the system when running it.
For Windows, it is not needed to have Qt installed in the system to run your program. The needed dll files are copied to the output folder to make it self contained. However qt is still dynamically linked.

## Platform support

This project currently only works on Linux and Windows, eventually Mac OS X might be supported as well.

## Usage

You can either copy the qt.BUILD and qt.bzl files into your project, add this project as a submodule if you're using git or use a git_repository rule to fetch the rules.

Configure your WORKSPACE to include the qt libraries:

```python
# WORKSPACE

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

git_repository(
    name = "com_limdor_rules_qt",
    remote = "https://github.com/limdor/bazel_rules_qt.git",
    branch = "master",
)

new_local_repository(
    name = "qt",
    build_file = "@com_limdor_rules_qt//:qt.BUILD",
    # May need configuring for your installation
    # For Qt5 on Ubuntu
    path = "/usr/include/x86_64-linux-gnu/qt5/",
    # For Qt 5.9.9 on Windows for Visual Studio 2015
    # path = "C:\\Qt\\5.9.9\\msvc2015_64\\",
)
```

Use the build rules provided by qt.bzl to build your project. See qt.bzl for which rules are available.

```python
# BUILD

load("@com_limdor_rules_qt//:qt.bzl", "qt_cc_library", "qt_ui_library")

qt_cc_library(
    name = "MyWidget",
    srcs = ["MyWidget.cc"],
    hdrs = ["MyWidget.h"],
    deps = ["@qt//:qt_widgets"],
)

qt_ui_library(
    name = "mainwindow",
    ui = "mainwindow.ui",
    deps = ["@qt//:qt_widgets"],
)

cc_binary(
    name = "main",
    srcs = ["main.cpp"],
    copts = ["-fpic"],
    deps = [
        "@qt//:qt_widgets",
        ":MyWidget",
        ":mainwindow",
    ],
)
```

## Credits

This is a fork of https://github.com/justbuchanan/bazel_rules_qt with changes to make it work on Windows.
