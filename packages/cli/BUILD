load("@build_bazel_rules_nodejs//:index.bzl", "pkg_npm")
load("//tools:index.bzl", "ts_compile")
load("//tools:jest.bzl", "jest_test")

PACKAGE_NAME = "cli"

pkg_npm(
    name = PACKAGE_NAME,
    package_name = "@formatjs/%s" % PACKAGE_NAME,
    srcs = [
        "LICENSE.md",
        "package.json",
    ],
    deps = [
        "bin/formatjs",
        ":dist",
    ],
)

SRCS = glob(["src/*.ts"]) + [
    "index.ts",
]

SRC_DEPS = [
    "//packages/babel-plugin-react-intl:dist",
    "@npm//@babel/core",
    "@npm//@types/node",
    "@npm//fs-extra",
    "@npm//@types/fs-extra",
    "//packages/ts-transformer:dist",
    "@npm//glob",
    "@npm//@types/glob",
    "@npm//commander",
    "@npm//loud-rejection",
    "@npm//chalk",
]

ts_compile(
    name = "dist",
    package_name = "@formatjs/%s" % PACKAGE_NAME,
    srcs = SRCS,
    deps = SRC_DEPS,
)

# TODO: Add integration tests
jest_test(
    name = "unit",
    srcs = [
        "package.json",
        "tests/extract/unit.test.ts",
    ] + SRCS,
    deps = [
        "@npm//eslint",
        "//packages/babel-plugin-react-intl:types",
        "//packages/ts-transformer:types",
        "@npm//rimraf",
        "@npm//@types/rimraf",
    ] + SRC_DEPS,
)