load("@build_bazel_rules_nodejs//:index.bzl", "generated_file_test", "pkg_npm")
load("//tools:index.bzl", "check_format", "ts_compile")
load("//tools:jest.bzl", "jest_test")

PACKAGE_NAME = "cli"

pkg_npm(
    name = PACKAGE_NAME,
    package_name = "@formatjs/%s" % PACKAGE_NAME,
    srcs = [
        "LICENSE.md",
        "README.md",
    ],
    deps = [
        "bin/formatjs",
        ":dist",
    ],
)

alias(
    name = "publish",
    actual = "%s.publish" % PACKAGE_NAME,
)

alias(
    name = "pack",
    actual = "%s.pack" % PACKAGE_NAME,
)

SRCS = glob(["src/**/*.ts"]) + [
    "index.ts",
]

SRC_DEPS = [
    "@npm//@types/estree",
    "@npm//@types/fs-extra",
    "@npm//@types/json-stable-stringify",
    "@npm//@types/node",
    "@npm//@vue/compiler-sfc",
    "@npm//chalk",
    "@npm//commander",
    "@npm//fast-glob",
    "@npm//fs-extra",
    "@npm//json-stable-stringify",
    "@npm//loud-rejection",
    "//packages/icu-messageformat-parser:dist",
    "//packages/ts-transformer:dist",
]

ts_compile(
    name = "dist",
    package_name = "@formatjs/%s" % PACKAGE_NAME,
    srcs = SRCS,
    deps = SRC_DEPS,
)

jest_test(
    name = "unit",
    srcs = [
        "package.json",
        "tests/extract/unit.test.ts",
        "tests/extract/vue_extractor.test.ts",
        "tests/extract/__snapshots__/unit.test.ts.snap",
    ] + SRCS,
    deps = [
        "//packages/icu-messageformat-parser:types",
        "//packages/ts-transformer:types",
        "@npm//rimraf",
        "@npm//@types/rimraf",
    ] + SRC_DEPS,
)

jest_test(
    name = "integration",
    size = "large",
    srcs = [
        "package.json",
        "bin/formatjs",
    ] + SRCS + glob(
        [
            "tests/**/*",
        ],
        exclude = ["tests/extract/unit.test.ts"],
    ),
    flaky = True,
    deps = [
        "//packages/icu-messageformat-parser:types",
        "//packages/ts-transformer:types",
        "@npm//rimraf",
        "@npm//@types/rimraf",
        ":dist",
    ] + SRC_DEPS,
)

generated_file_test(
    name = "tsconfig_json",
    src = "tsconfig.json",
    generated = "//tools:tsconfig.golden.json",
)

check_format(
    name = "prettier",
    srcs = glob(
        [
            "**/*",
        ],
        exclude = [
            "CHANGELOG.md",
            # Intentional err
            "tests/extract/typescript/err.tsx",
            "tests/*/__snapshots__/*",
        ],
    ),
)
