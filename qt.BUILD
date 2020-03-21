load("@rules_cc//cc:defs.bzl", "cc_import", "cc_library")

cc_import(
    name = "qt_core_private",
    hdrs = glob(["include/QtCore/**"]),
    interface_library = "lib/Qt5Core.lib",
    shared_library = "bin/Qt5Core.dll",
)

cc_library(
    name = "qt_core",
    hdrs = glob(["include/QtCore/**"]),
    includes = ["include"],
    visibility = ["//visibility:public"],
    deps = [":qt_core_private"],
)

cc_import(
    name = "qt_widgets_private",
    hdrs = glob(["include/QtWidgets/**"]),
    interface_library = "lib/Qt5Widgets.lib",
    shared_library = "bin/Qt5Widgets.dll",
)

cc_library(
    name = "qt_widgets",
    hdrs = glob(["include/QtWidgets/**"]),
    includes = ["include"],
    visibility = ["//visibility:public"],
    deps = [":qt_widgets_private"],
)

cc_import(
    name = "qt_opengl_private",
    hdrs = glob(["include/QtOpenGL/**"]),
    interface_library = "lib/Qt5OpenGL.lib",
    shared_library = "bin/Qt5OpenGL.dll",
)

cc_library(
    name = "qt_opengl",
    hdrs = glob(["include/QtOpenGL/**"]),
    includes = ["include"],
    visibility = ["//visibility:public"],
    deps = [":qt_opengl_private"],
)

cc_import(
    name = "qt_gui_private",
    hdrs = glob(["include/QtGui/**"]),
    interface_library = "lib/Qt5Gui.lib",
    shared_library = "bin/Qt5Gui.dll",
)

cc_library(
    name = "qt_gui",
    hdrs = glob(["include/QtGui/**"]),
    includes = ["include"],
    visibility = ["//visibility:public"],
    deps = [":qt_gui_private"],
)

filegroup(
    name = "uic",
    srcs = ["bin/uic.exe"],
    visibility = ["//visibility:public"],
)

filegroup(
    name = "moc",
    srcs = ["bin/moc.exe"],
    visibility = ["//visibility:public"],
)
