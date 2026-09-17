load("@rules_python//python:defs.bzl", "py_library", "py_test")

py_library(
    name = "typing_extensions",
    srcs = ["typing_extensions/src/typing_extensions.py"],
    imports = ["typing_extensions/src"],
    visibility = ["//visibility:public"],
)

py_test(
    name = "typing_extensions_test",
    srcs = [
        "typing_extensions/src/_typed_dict_test_helper.py",
        "typing_extensions/src/test_typing_extensions.py",
    ],
    main = "typing_extensions/src/test_typing_extensions.py",
    imports = ["typing_extensions/src"],
    deps = [":typing_extensions"],
)
