workspace(name = "rules_haskell")

load("//haskell:repositories.bzl", "rules_haskell_dependencies")

rules_haskell_dependencies()

load("//:non_module_deps.bzl", "repositories")

repositories(bzlmod = False)

load("@os_info//:os_info.bzl", "is_linux", "is_nix_shell", "is_windows")

# bazel dependencies
load("@aspect_rules_js//js:repositories.bzl", "rules_js_dependencies")

rules_js_dependencies()

load("@bazel_skylib//:workspace.bzl", "bazel_skylib_workspace")

bazel_skylib_workspace()

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@rules_haskell//haskell:cabal.bzl", "stack_snapshot")

http_archive(
    name = "rules_proto",
    sha256 = "36476f17a78a4c495b9a9e70bd92d182e6e78db476d90c74bac1f5f19f0d6d04",
    strip_prefix = "rules_proto-fcad4680fee127dbd8344e6a961a28eef5820ef4",
    urls = ["https://github.com/bazelbuild/rules_proto/archive/fcad4680fee127dbd8344e6a961a28eef5820ef4.tar.gz"],
)

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies", "rules_proto_toolchains")

rules_proto_dependencies()

rules_proto_toolchains()

# For buildifier
# starting from 0.29, rules_go requires bazel >= 4.2.0
http_archive(
    name = "io_bazel_rules_go",
    sha256 = "8e968b5fcea1d2d64071872b12737bbb5514524ee5f0a4f54f5920266c261acb",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.28.0/rules_go-v0.28.0.zip",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.28.0/rules_go-v0.28.0.zip",
    ],
)

load("non_module_deps_1.bzl", repositories_1 = "repositories")

repositories_1(bzlmod = False)

load("non_module_deps_2.bzl", repositories_2 = "repositories")

repositories_2(bzlmod = False)

load("@rules_haskell_npm//:repositories.bzl", "npm_repositories")

npm_repositories()

register_toolchains(
    "//tests:c2hs-toolchain",
    "//tests:doctest-toolchain",
    "//tests:protobuf-toolchain",
    "//tests:protobuf-toolchain-osx_arm64",
)

load("@bazel_tools//tools/build_defs/repo:jvm.bzl", "jvm_maven_import_external")

jvm_maven_import_external(
    name = "org_apache_spark_spark_core_2_10",
    artifact = "org.apache.spark:spark-core_2.10:1.6.0",
    artifact_sha256 = "28aad0602a5eea97e9cfed3a7c5f2934cd5afefdb7f7c1d871bb07985453ea6e",
    licenses = ["notice"],
    server_urls = ["https://repo.maven.apache.org/maven2"],
)

load("@io_bazel_stardoc//:setup.bzl", "stardoc_repositories")

stardoc_repositories()

register_toolchains(
    "@rules_haskell//docs/pandoc:nixpkgs",
    "@rules_haskell//docs/pandoc:linux",
    "@rules_haskell//docs/pandoc:macos",
)

# A repository that generates the Go SDK imports, see ./tools/go_sdk/README
local_repository(
    name = "go_sdk_repo",
    path = "tools/go_sdk",
)

load(
    "@io_bazel_rules_go//go:deps.bzl",
    "go_register_toolchains",
    "go_rules_dependencies",
)

go_rules_dependencies()

# If in nix-shell, use the Go SDK provided by Nix.
# Otherwise, ask Bazel to download a Go SDK.
go_register_toolchains(version = "host") if is_nix_shell else go_register_toolchains(version = "1.16.2")

load("@com_github_bazelbuild_buildtools//buildifier:deps.bzl", "buildifier_dependencies")

buildifier_dependencies()

load("@contrib_rules_bazel_integration_test//bazel_integration_test:deps.bzl", "bazel_integration_test_rules_dependencies")

bazel_integration_test_rules_dependencies()

load("@cgrindel_bazel_starlib//:deps.bzl", "bazel_starlib_dependencies")

bazel_starlib_dependencies()

# For profiling
# Required to make use of `bazel build --profile`.

# Dummy target //external:python_headers.
# See https://github.com/protocolbuffers/protobuf/blob/d9ccd0c0e6bbda9bf4476088eeb46b02d7dcd327/util/python/BUILD
bind(
    name = "python_headers",
    actual = "@com_google_protobuf//util/python:python_headers",
)
