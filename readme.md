# Bazel rules for Qt [![ci status](https://circleci.com/gh/limdor/bazel_rules_qt.png)](https://app.circleci.com/pipelines/github/limdor/bazel_rules_qt?branch=master)

These bazel rules and BUILD targets make it easy to use Qt from C++ projects built with bazel.

Note that unlike the original bazel_rules_qt, it is not needed to have Qt installed in the system to run your program. The needed dll files are copied to the output folder to make it self contained. However qt is still dynamically linked.

## Usage

You can either copy the qt.BUILD and qt.bzl files into your project, add this project as a submodule if you're using git or use a git_repository rule to fetch the rules.

Configure your WORKSPACE to include the qt libraries:

```python
# WORKSPACE

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

git_repository(
    name = "bazel_rules_qt",
    remote = "https://github.com/limdor/bazel_rules_qt.git",
    branch = "master",
)

new_local_repository(
    name = "qt",
    build_file = "@bazel_rules_qt//:qt.BUILD",
    path = "C:\\Qt\\5.9.9\\msvc2015_64\\", # May need configuring for your installation
)
```

Use the build rules provided by qt.bzl to build your project. See qt.bzl for which rules are available.

```python
# BUILD

load("@bazel_rules_qt//:qt.bzl", "qt_cc_library", "qt_ui_library")

qt_cc_library(
    name = "MyWidget",
    src = "MyWidget.cc",
    hdr = "MyWidget.h",
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
