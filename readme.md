# Bazel rules for Qt [![ci status](https://circleci.com/gh/justbuchanan/bazel_rules_qt.png?circle-token=9077bf6ecc5554e3ddbdc4d3947784460eb1df72)](https://app.circleci.com/pipelines/github/justbuchanan/bazel_rules_qt?branch=master)

These bazel rules and BUILD targets make it easy to use Qt from C++ projects built with bazel.

Note that unlike many libraries used through bazel, qt is dynamically linked, meaning that the qt-dependent programs you build with bazel will use the qt libraries installed by the system package manager. Thus the users of your programs will also need to install qt.

## Usage

You can either copy the qt.BUILD and qt.bzl files into your project, add this project as a submodule if you're using git or use a git_repository rule to fetch the rules.

Configure your WORKSPACE to include the qt libraries:

```python
# WORKSPACE

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

git_repository(
    name = "bazel_rules_qt",
    remote = "https://github.com/justbuchanan/bazel_rules_qt.git",
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

This is a fork of https://github.com/bbreslauer/qt-bazel-example with many modifications.
