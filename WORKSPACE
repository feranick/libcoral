# Copyright 2020 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
workspace(name = "libcoral")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# Configure libedgetpu and downstream libraries (TF and Crosstool).
local_repository(
    name = "libedgetpu",
    path = "libedgetpu",
)

# ==================================================================

# Add definition of tensorflow version 2.15.0
http_archive(
        name = "org_tensorflow",
        urls = [
            "https://github.com/tensorflow/tensorflow/archive/6887368d6d46223f460358323c4b76d61d1558a8.tar.gz",
        ],
        sha256 = "bb25fa4574e42ea4d452979e1d2ba3b86b39569d6b8106a846a238b880d73652",
        strip_prefix = "tensorflow-" + "6887368d6d46223f460358323c4b76d61d1558a8",
        )

http_archive(
    name = "bazel_skylib",
    sha256 = "74d544d96f4a5bb630d465ca8bbcfe231e3594e5aae57e1edbf17a6eb3ca2506",
    urls = [
        "https://storage.googleapis.com/mirror.tensorflow.org/github.com/bazelbuild/bazel-skylib/releases/download/1.3.0/bazel-skylib-1.3.0.tar.gz",
        "https://github.com/bazelbuild/bazel-skylib/releases/download/1.3.0/bazel-skylib-1.3.0.tar.gz",
    ],
)

http_archive(
    name = "rules_python",
    sha256 = "c68bdc4fbec25de5b5493b8819cfc877c4ea299c0dcb15c244c5a00208cde311",
    strip_prefix = "rules_python-0.31.0",
    url = "https://github.com/bazelbuild/rules_python/releases/download/0.31.0/rules_python-0.31.0.tar.gz",
)

load("@rules_python//python:repositories.bzl", "py_repositories")

py_repositories()

load("@rules_python//python:repositories.bzl", "python_register_toolchains")
load(
    "@org_tensorflow//tensorflow/tools/toolchains/python:python_repo.bzl",
    "python_repository",
)

python_repository(name = "python_version_repo")

load("@python_version_repo//:py_version.bzl", "HERMETIC_PYTHON_VERSION")

python_register_toolchains(
    name = "python",
    ignore_root_user_error = True,
    python_version = HERMETIC_PYTHON_VERSION,
)

load("@python//:defs.bzl", "interpreter")
load("@rules_python//python:pip.bzl", "package_annotation")

# ==================================================================

load("@libedgetpu//:workspace.bzl", "libedgetpu_dependencies")
libedgetpu_dependencies()

load("@org_tensorflow//tensorflow:workspace3.bzl", "tf_workspace3")
tf_workspace3()

load("@org_tensorflow//tensorflow:workspace2.bzl", "tf_workspace2")
tf_workspace2()

load("@org_tensorflow//tensorflow:workspace1.bzl", "tf_workspace1")
tf_workspace1()

load("@org_tensorflow//tensorflow:workspace0.bzl", "tf_workspace0")
tf_workspace0()

load("@coral_crosstool//:configure.bzl", "cc_crosstool")
cc_crosstool(name = "crosstool", cpp_version = "c++17")

# External Dependencies
http_archive(
    name = "com_google_glog",
    sha256 = "6fc352c434018b11ad312cd3b56be3597b4c6b88480f7bd4e18b3a3b2cf961aa",
    strip_prefix = "glog-3ba8976592274bc1f907c402ce22558011d6fc5e",
    urls = [
        "https://github.com/google/glog/archive/3ba8976592274bc1f907c402ce22558011d6fc5e.tar.gz",
    ],
    build_file_content = """
licenses(['notice'])
exports_files(['CMakeLists.txt'])
load(':bazel/glog.bzl', 'glog_library')
glog_library(with_gflags=0)
""",
)

http_archive(
  name = "com_github_google_benchmark",
  sha256 = "8e7b955f04bc6984e4f14074d0d191474f76a6c8e849e04a9dced49bc975f2d4",
  strip_prefix = "benchmark-344117638c8ff7e239044fd0fa7085839fc03021",
  urls = [
    "https://github.com/google/benchmark/archive/344117638c8ff7e239044fd0fa7085839fc03021.tar.gz"
  ],
)

# Local repos
new_local_repository(
    name = "test_data",
    path = "test_data",
    build_file = "test_data/BUILD"
)

new_local_repository(
    name = "glog",
    path = "third_party/glog",
    build_file = "third_party/glog/BUILD",
)

new_local_repository(
    name = "system_linux",
    build_file = "third_party/system_linux/BUILD",
    path = "/usr/system_libs",
)

