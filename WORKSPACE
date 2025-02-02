# Copyright 2016-17, OpenCensus Authors
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

workspace(name = "opencensus_proto")

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# Import grpc for cc_grpc_library().
http_archive(
    name = "com_github_grpc_grpc",
    sha256 = "54130a7fa3dae57ed148f24cddcc91ff56e8023ed3d1e44cff4e1a922406087d",
    strip_prefix = "grpc-809e7c951358a80182d7126b255c3a40881fb3fa",
    urls = ["https://github.com/grpc/grpc/archive/809e7c951358a80182d7126b255c3a40881fb3fa.zip"],
)

load("@com_github_grpc_grpc//bazel:grpc_deps.bzl", "grpc_deps")

grpc_deps()

# go rules related
git_repository(
    name = "io_bazel_rules_go",
    remote = "https://github.com/bazelbuild/rules_go.git",
    # we need rules_go of tag 0.12.0 or greater
    # which includes golang protobuf that supports "paths=source_relative"
    # the "paths=source_relative" option for protoc-gen-go makes bazel works as expect
    # when go_package was declared in proto files
    # see https://github.com/bazelbuild/rules_go/blob/release-0.12/go/private/repositories.bzl#L75
    # for the included golang protobuf version and
    # see https://github.com/golang/protobuf/pull/544 for "paths=source_relative" usage
    tag = "0.19.1",
)

http_archive(
    name = "io_bazel_rules_go",
    urls = [
        "https://storage.googleapis.com/bazel-mirror/github.com/bazelbuild/rules_go/releases/download/0.19.1/rules_go-0.19.1.tar.gz",
        "https://github.com/bazelbuild/rules_go/releases/download/0.19.1/rules_go-0.19.1.tar.gz",
    ],
    sha256 = "8df59f11fb697743cbb3f26cfb8750395f30471e9eabde0d174c3aebc7a1cd39",
)

http_archive(
    name = "bazel_gazelle",
    urls = [
        "https://storage.googleapis.com/bazel-mirror/github.com/bazelbuild/bazel-gazelle/releases/download/0.18.1/bazel-gazelle-0.18.1.tar.gz",
        "https://github.com/bazelbuild/bazel-gazelle/releases/download/0.18.1/bazel-gazelle-0.18.1.tar.gz",
    ],
    sha256 = "be9296bfd64882e3c08e3283c58fcb461fa6dd3c171764fcc4cf322f60615a9b",
)

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies", "go_repository")

gazelle_dependencies(go_sdk = "go_sdk")

load(
    "@io_bazel_rules_go//go:deps.bzl",
    "go_download_sdk",
    "go_register_toolchains",
    "go_rules_dependencies",
)

go_download_sdk(
    name = "go_sdk",
    sdks = {
        "linux_amd64": ("go1.12.7.linux-amd64.tar.gz", "66d83bfb5a9ede000e33c6579a91a29e6b101829ad41fffb5c5bb6c900e109d9"),
    },
)

load("@io_bazel_rules_go//go:deps.bzl", "go_rules_dependencies", "go_register_toolchains")
go_rules_dependencies()
go_register_toolchains()

# py rules related
http_archive(
    name = "six_archive",
    build_file = "@com_google_protobuf//:six.BUILD",
    sha256 = "105f8d68616f8248e24bf0e9372ef04d3cc10104f1980f54d57b2ce73a5ad56a",
    urls = ["https://pypi.python.org/packages/source/s/six/six-1.10.0.tar.gz#md5=34eed507548117b2ab523ab14b2f8b55"],
)

bind(
    name = "six",
    actual = "@six_archive//:six",
)
