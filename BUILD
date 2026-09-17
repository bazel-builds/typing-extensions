load("@rules_license//rules:license.bzl", "license")
load("@rules_license//rules:package_info.bzl", "package_info")
load("@rules_python//python:defs.bzl", "py_library", "py_test")

package(default_applicable_licenses = [":license"])

package_info(
    name = "package_info",
    package_name = "typing_extensions",
    package_url = "https://github.com/python/typing_extensions",
    package_version = "4.16.0",
)

license(
    name = "license",
    license_kinds = ["@rules_license//licenses/spdx:PSF-2.0"],
    package_name = "typing_extensions",
)

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
