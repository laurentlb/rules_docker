# Copyright 2017 The Bazel Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load(
    "@bazel_tools//tools/build_rules:test_rules.bzl",
    "file_test",
)
load("//container:bundle.bzl", "container_bundle")
load(
    "//container:container.bzl",
    "container_image",
    "container_import",
    "container_push",
)
load(
    "//container:new_push.bzl",
    "new_container_push",
)
load("//contrib:push-all.bzl", docker_push_all = "docker_push")
load("//contrib:test.bzl", "container_test")
load(":apple.bzl", "create_banana_directory")

package(default_visibility = ["//visibility:public"])

container_test(
    name = "structure_test",
    configs = ["//tests/container/configs:test.yaml"],
    image = "//testdata:link_with_files_base",
)

container_image(
    name = "deb_image_with_dpkgs",
    debs = [
        "//testdata:pkg1.deb",
        "//testdata:pkg2.deb",
        "//testdata:pkg_control_gz.deb",
        "//testdata:pkg_control_xz.deb",
        # TODO: (tejaldesai) Once we figure out how to selectively disable tests using build_tag_filters
        # ":busybox-static_ubuntu1_amd64.deb",
    ],
    workdir = "/test",
)

container_test(
    name = "deb_image_with_dpkgs_test",
    configs = ["//tests/container/configs:deb_image_with_dpkgs.yaml"],
    image = ":deb_image_with_dpkgs",
)

container_image(
    name = "set_cmd",
    base = "@distroless_fixed_id//image",
    cmd = ["foo"],
)

container_test(
    name = "set_cmd_test",
    configs = ["//tests/container/configs:set_cmd.yaml"],
    image = ":set_cmd",
)

container_test(
    name = "set_cmd_test_host",
    configs = ["//tests/container/configs:set_cmd.yaml"],
    driver = "host",
    image = ":set_cmd",
)

container_image(
    name = "set_entrypoint",
    base = "@distroless_fixed_id//image",
    entrypoint = ["bar"],
)

container_test(
    name = "set_entrypoint_test",
    configs = ["//tests/container/configs:set_entrypoint.yaml"],
    image = ":set_entrypoint",
)

container_image(
    name = "set_cmd_and_entrypoint",
    base = "@distroless_fixed_id//image",
    cmd = ["foo"],
    entrypoint = ["bar"],
)

container_test(
    name = "set_cmd_and_entrypoint_test",
    configs = ["//tests/container/configs:set_cmd_and_entrypoint.yaml"],
    image = ":set_cmd_and_entrypoint",
)

container_image(
    name = "base_with_cmd_and_entrypoint",
    base = ":set_cmd_and_entrypoint",
)

container_test(
    name = "base_with_cmd_and_entrypoint_test",
    configs = ["//tests/container/configs:set_cmd_and_entrypoint.yaml"],
    image = ":base_with_cmd_and_entrypoint",
)

container_image(
    name = "null_cmd_and_entrypoint_none",
    base = ":set_cmd_and_entrypoint",
    cmd = None,
    entrypoint = None,
)

container_test(
    name = "null_cmd_and_entrypoint_none_test",
    configs = ["//tests/container/configs:null_cmd_and_entrypoint.yaml"],
    image = ":null_cmd_and_entrypoint_none",
)

container_image(
    name = "null_cmd_and_entrypoint_empty_str",
    base = ":set_cmd_and_entrypoint",
    cmd = "",
    entrypoint = "",
)

container_test(
    name = "null_cmd_and_entrypoint_empty_str_test",
    configs = ["//tests/container/configs:null_cmd_and_entrypoint.yaml"],
    image = ":null_cmd_and_entrypoint_empty_str",
)

container_image(
    name = "null_cmd_and_entrypoint_empty_list",
    base = ":set_cmd_and_entrypoint",
    cmd = [],
    entrypoint = [],
)

container_test(
    name = "null_cmd_and_entrypoint_empty_list_test",
    configs = ["//tests/container/configs:null_cmd_and_entrypoint.yaml"],
    image = ":null_cmd_and_entrypoint_empty_list",
)

container_image(
    name = "set_cmd_and_entrypoint_null_on_base",
    base = ":null_cmd_and_entrypoint_none",
    cmd = ["foo"],
    entrypoint = ["bar"],
)

container_test(
    name = "set_cmd_and_entrypoint_null_on_base_test",
    configs = ["//tests/container/configs:set_cmd_and_entrypoint.yaml"],
    image = ":set_cmd_and_entrypoint_null_on_base",
)

container_import(
    name = "import_windows_base_image",
    config = "windowsservercore.1803.config.json",
    layers = [],
    manifest = "windowsservercore.1803.manifest.json",
)

container_image(
    name = "basic_windows_image",
    base = ":import_windows_base_image",
    cmd = ["echo bar"],
    operating_system = "windows",
)

container_test(
    name = "basic_windows_image_test",
    configs = ["//tests/container/configs:windows_image.yaml"],
    driver = "tar",
    image = ":basic_windows_image",
)

# The following targets are not imported.
# These images are only available and tested on GitHub.
# BEGIN_DO_NOT_IMPORT
container_test(
    name = "alpine_amd64_image_test",
    configs = ["//tests/container/configs:alpine_amd64.yaml"],
    image = "@alpine_linux_amd64//image",
)

container_test(
    name = "alpine_arm_image_test",
    configs = ["//tests/container/configs:alpine_arm.yaml"],
    image = "@alpine_linux_armv6//image",
)

container_test(
    name = "alpine_ppc64le_image_test",
    configs = ["//tests/container/configs:alpine_ppc64le.yaml"],
    image = "@alpine_linux_ppc64le//image",
)
# END_DO_NOT_IMPORT

# The following targets are not imported.
# Digest outputs are only available and tested on GitHub.
# BEGIN_DO_NOT_IMPORT
file_test(
    name = "test_digest_output1",
    content = "sha256:bfe2a9d4b2c37ceaa0b5db475c54f356846f4ebbb70a8e8c4b10525f9270ac72",
    file = ":deb_image_with_dpkgs.digest",
)

file_test(
    name = "test_digest_output2",
    content = "sha256:437978f37315238cca48d540a5dbdcfbc8a6b617f17a04e6be931718414abbd6",
    file = ":null_cmd_and_entrypoint_none.digest",
)
# END_DO_NOT_IMPORT

container_image(
    name = "set_env_csv",
    base = "@distroless_fixed_id//image",
    cmd = ["foo"],
    env = {
        "NO_PROXY": "127.0.0.1,localhost",
    },
)

container_test(
    name = "set_env_csv_test",
    configs = ["//tests/container/configs:set_env_csv.yaml"],
    image = ":set_env_csv",
)

# Image with Bazel Make variables in the env field.
container_image(
    name = "set_env_make_vars",
    base = "@distroless_fixed_id//image",
    env = {
        "$(ENV_KEY)": "$(ENV_VALUE)",
    },
)

# This test checks that Bazel Make variables are expanded in the env field.
container_test(
    name = "set_env_make_vars_test",
    configs = ["//tests/container/configs:set_env_make_vars.yaml"],
    image = ":set_env_make_vars",
)

# The following targets are not imported.
# Targets using special characters are only available and tested on GitHub.
# BEGIN_DO_NOT_IMPORT
genrule(
    name = "special_characters_file",
    outs = ["gen= foo.out"],
    cmd = "echo generated > \"$@\"",
)

container_image(
    name = "special_characters",
    base = "@distroless_fixed_id//image",
    directory = "root= dir",
    empty_dirs = ["/empty= dir"],
    empty_files = ["/empty= file"],
    files = [":special_characters_file"],
    symlinks = {"foo= symlink": "root= dir/gen= foo.out"},
)
# END_DO_NOT_IMPORT

container_test(
    name = "special_characters_test",
    configs = ["//tests/container/configs:special_characters.yaml"],
    driver = "tar",
    image = ":special_characters",
)

container_push(
    name = "push_test",
    format = "Docker",
    image = ":set_cmd",
    registry = "localhost:5000",
    repository = "docker/test",
    tag = "test",
    tags = ["manual"],
)

genrule(
    name = "push_tag_file",
    srcs = [],
    outs = ["test.tag"],
    cmd = """echo test > $@""",
    output_to_bindir = 1,
)

container_push(
    name = "push_tag_file_test",
    format = "Docker",
    image = ":set_cmd",
    registry = "localhost:5000",
    repository = "docker/test",
    tag_file = ":test.tag",
    tags = ["manual"],
)

container_push(
    name = "push_stamped_test",
    format = "Docker",
    image = ":set_cmd",
    registry = "localhost:5000",
    repository = "docker/test/{BUILD_USER}",
    tag = "{BUILD_USER}",
    tags = ["manual"],
)

container_image(
    name = "alpine_with_custom_attr",
    base = "@new_alpine_linux_amd64//image",
    cmd = ["Hello World!"],
    entrypoint = ["echo"],
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
    layers = [
        "//testdata:env_layer",
        "//testdata:tars_in_layer",
        "//testdata:files_in_layer",
    ],
)

create_banana_directory(
    name = "banana_directory",
)

container_image(
    name = "stripped_directory_name",
    data_path = "banana",
    files = [":banana_directory"],
)

container_test(
    name = "stripped_directory_name_test",
    configs = ["//tests/container/configs:stripped_directory_name.yaml"],
    image = ":stripped_directory_name",
)

container_bundle(
    name = "three_images_bundle",
    images = {
        "localhost:5000/image0:latest": "//testdata:base_with_entrypoint",
        "localhost:5000/image1:latest": "//testdata:link_with_files_base",
        "localhost:5000/image2:latest": "//testdata:with_double_env",
    },
)

docker_push_all(
    name = "test_docker_push_three_images_bundle",
    bundle = ":three_images_bundle",
)

py_test(
    name = "build_tar_test",
    srcs = ["build_tar_test.py"],
    python_version = "PY2",
    deps = ["//container:build_tar_lib"],
)

# The following targets are not imported.
# new_container_push targets are only available and tested on GitHub.
# BEGIN_DO_NOT_IMPORT
new_container_push(
    name = "new_push_test_oci",
    format = "oci",
    image = "//container/go/pkg/oci:oci_test_index1",
    registry = "localhost:5000",
    repository = "docker/test",
    tag = "test",
    tags = ["manual"],
)

new_container_push(
    name = "new_push_test_oci_from_new_puller",
    format = "oci",
    image = "@new_alpine_linux_amd64//image-oci",
    registry = "localhost:5000",
    repository = "docker/test",
    tag = "test",
    tags = ["manual"],
)

new_container_push(
    name = "new_push_test_legacy_from_container_img",
    format = "legacy",
    image = ":alpine_with_custom_attr",
    registry = "localhost:5000",
    repository = "docker/test",
    tag = "test",
    tags = ["manual"],
)

new_container_push(
    name = "new_push_test_legacy_from_container_img_with_auth",
    format = "legacy",
    image = ":set_cmd_and_entrypoint",
    registry = "localhost:5000",
    repository = "docker/test",
    tag = "test",
    tags = ["manual"],
)

new_container_push(
    name = "new_push_test_legacy_from_new_puller",
    format = "legacy",
    image = "@new_alpine_linux_ppc64le//image",
    registry = "localhost:5000",
    repository = "docker/test",
    tag = "test",
    tags = ["manual"],
)

new_container_push(
    name = "new_push_test_legacy_from_old_puller",
    format = "legacy",
    image = "@alpine_linux_armv6//image",
    registry = "localhost:5000",
    repository = "docker/test",
    tag = "test",
    tags = ["manual"],
)

new_container_push(
    name = "new_push_test_tar",
    format = "docker",
    image = "@new_alpine_linux_ppc64le//image:image.tar",
    registry = "localhost:5000",
    repository = "docker/test",
    tag = "test",
    tags = ["manual"],
)

new_container_push(
    name = "new_push_test_old_puller_tar",
    format = "docker",
    image = "@alpine_linux_ppc64le//image:image.tar",
    registry = "localhost:5000",
    repository = "docker/test",
    tag = "test",
    tags = ["manual"],
)

new_container_push(
    name = "new_push_test_oci_tag_file",
    format = "oci",
    image = "//container/go/pkg/oci:oci_test_index1",
    registry = "localhost:5000",
    repository = "docker/test",
    tag_file = ":test.tag",
    tags = ["manual"],
)

new_container_push(
    name = "new_push_test_legacy_tag_file",
    format = "legacy",
    image = ":set_cmd",
    registry = "localhost:5000",
    repository = "docker/test",
    tag_file = ":test.tag",
    tags = ["manual"],
)

# END_DO_NOT_IMPORT

# The following targets are not imported.
# Digest outputs are only available and tested on GitHub.
# BEGIN_DO_NOT_IMPORT
file_test(
    name = "test_push_digest_output",
    content = "sha256:3b7f28bf8caefd13d4a22513727f3e8cd8d3a464c72df387f0fd7a74a0f82f2b",
    file = ":push_test.digest",
)

file_test(
    name = "distoless_fixed_id_digest_test",
    content = "sha256:a26dde6863dd8b0417d7060c990abe85c1d2481541568445e82b46de9452cf0c",
    file = "@distroless_fixed_id//image:digest",
)

file_test(
    name = "distroless_fixed_id_image_digest_test",
    content = "sha256:a26dde6863dd8b0417d7060c990abe85c1d2481541568445e82b46de9452cf0c",
    file = "@distroless_fixed_id//image:image.digest",
)

file_test(
    name = "k8s_pause_arm64_digest_test",
    content = "sha256:f365626a556e58189fc21d099fc64603db0f440bff07f77c740989515c544a39",
    file = "@k8s_pause_arm64//image:digest",
)

file_test(
    name = "alpine_custom_attr_digest_test",
    content = "sha256:0d91921356df81fd2015764032b380fd97315b08967c6e7c5829b8729ac670d2",
    file = ":new_push_test_legacy_from_container_img.digest",
)

file_test(
    name = "new_alpine_linux_armv6_image_tar_digest_test",
    content = "sha256:f209a250c03b2b96b9adb685a68e799f81bad44c8d500812dd8a19f32ece189e",
    file = ":new_alpine_linux_armv6_image_tar.digest",
)

# END_DO_NOT_IMPORT

# The following targets are not imported.
# new_container_pull targets are only available and tested on GitHub.
# BEGIN_DO_NOT_IMPORT
file_test(
    name = "new_alpine_linux_armv6_test_index_json",
    content = """{
   "schemaVersion": 2,
   "manifests": [
      {
         "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
         "size": 528,
         "digest": "sha256:f29c3d10359dd0e6d0c11e4f715735b678c0ab03a7ac4565b4b6c08980f6213b"
      }
   ]
}""",
    file = "@new_alpine_linux_armv6_fixed_id//image-oci:index.json",
)

file_test(
    name = "new_distroless_base_test_index_json",
    content = """{
   "schemaVersion": 2,
   "manifests": [
      {
         "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
         "size": 444,
         "digest": "sha256:a26dde6863dd8b0417d7060c990abe85c1d2481541568445e82b46de9452cf0c"
      }
   ]
}""",
    file = "@new_distroless_base_fixed_id//image-oci:index.json",
)

file_test(
    name = "new_alpine_linux_armv6_test_oci_layout",
    content = """{
    "imageLayoutVersion": "1.0.0"
}""",
    file = "@new_alpine_linux_armv6_fixed_id//image-oci:oci-layout",
)

file_test(
    name = "new_distroless_base_test_oci_layout",
    content = """{
    "imageLayoutVersion": "1.0.0"
}""",
    file = "@new_distroless_base_fixed_id//image-oci:oci-layout",
)

# Merely testing for the presence of the pulled image.tar for now.
file_test(
    name = "new_alpine_linux_armv6_tar_test_image_tar",
    file = "@new_alpine_linux_armv6_tar//image:image.tar",
    regexp = ".*",
)

file_test(
    name = "new_alpine_linux_armv6_oci_test_oci_layout",
    content = """{
    "imageLayoutVersion": "1.0.0"
}""",
    file = "@new_alpine_linux_armv6_fixed_id//image-oci:oci-layout",
)

# Testing for content of symlinks
file_test(
    name = "new_distroless_base_oci_test_config_symlink",
    file = "@new_alpine_linux_armv6_fixed_id//image:config.json",
    regexp = "{\"architecture\":\"arm\",.*",
)

file_test(
    name = "new_distroless_base_oci_test_0_tar_gz_symlink",
    file = "@new_alpine_linux_armv6_fixed_id//image:0.tar.gz",
    regexp = ".*",
)

# Testing all new_container_pull invocations with container_tests
# We are testing for 3 cases: 1) Presence of OCI image format only
#                             2) Presence of image.tar via container_import target
#                             3) Presence of image.tar via "docker" format option
#
# Three cases for alpine armv6 distro
container_image(
    name = "new_alpine_linux_armv6_image_tar",
    base = "@new_alpine_linux_armv6_tar//image:image.tar",
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
)

container_image(
    name = "new_alpine_linux_armv6_image_oci_tar",
    base = "@new_alpine_linux_armv6_fixed_id//image:image.tar",
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
)

container_image(
    name = "new_alpine_linux_armv6_image_oci",
    base = "@new_alpine_linux_armv6_fixed_id//image:image",
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
)

container_test(
    name = "new_alpine_linux_armv6_image_tar_test",
    configs = ["//tests/container/configs:alpine_arm.yaml"],
    image = ":new_alpine_linux_armv6_image_tar",
)

container_test(
    name = "new_alpine_linux_armv6_image_oci_tar_test",
    configs = ["//tests/container/configs:alpine_arm.yaml"],
    image = ":new_alpine_linux_armv6_image_oci_tar",
)

container_test(
    name = "new_alpine_linux_armv6_image_oci_test",
    configs = ["//tests/container/configs:alpine_arm.yaml"],
    image = ":new_alpine_linux_armv6_image_oci",
)

# Three cases for alpine amd64 distro
container_image(
    name = "new_alpine_linux_amd64_image_tar",
    base = "@new_alpine_linux_amd64_tar//image:image.tar",
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
)

container_image(
    name = "new_alpine_linux_amd64_image_oci_tar",
    base = "@new_alpine_linux_amd64//image:image.tar",
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
)

container_image(
    name = "new_alpine_linux_amd64_image_oci",
    base = "@new_alpine_linux_amd64//image:image",
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
)

container_test(
    name = "new_alpine_linux_amd64_image_tar_test",
    configs = ["//tests/container/configs:alpine_amd64.yaml"],
    image = ":new_alpine_linux_amd64_image_tar",
)

container_test(
    name = "new_alpine_linux_amd64_image_oci_tar_test",
    configs = ["//tests/container/configs:alpine_amd64.yaml"],
    image = ":new_alpine_linux_amd64_image_oci_tar",
)

container_test(
    name = "new_alpine_linux_amd64_image_oci_test",
    configs = ["//tests/container/configs:alpine_amd64.yaml"],
    image = ":new_alpine_linux_amd64_image_oci",
)

# Three cases for alpine ppc64le distro
container_image(
    name = "new_alpine_linux_ppc64le_image_tar",
    base = "@new_alpine_linux_ppc64le_tar//image:image.tar",
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
)

container_image(
    name = "new_alpine_linux_ppc64le_image_oci_tar",
    base = "@new_alpine_linux_ppc64le//image:image.tar",
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
)

container_image(
    name = "new_alpine_linux_ppc64le_image_oci",
    base = "@new_alpine_linux_ppc64le//image:image",
    files = [
        "//testdata:bar",
        "//testdata:foo",
    ],
)

container_test(
    name = "new_alpine_linux_ppc64le_image_tar_test",
    configs = ["//tests/container/configs:alpine_ppc64le.yaml"],
    image = ":new_alpine_linux_ppc64le_image_tar",
)

container_test(
    name = "new_alpine_linux_ppc64le_image_oci_tar_test",
    configs = ["//tests/container/configs:alpine_ppc64le.yaml"],
    image = ":new_alpine_linux_ppc64le_image_oci_tar",
)

container_test(
    name = "new_alpine_linux_ppc64le_image_oci_test",
    configs = ["//tests/container/configs:alpine_ppc64le.yaml"],
    image = ":new_alpine_linux_ppc64le_image_oci",
)
# END_DO_NOT_IMPORT

# The following targets are not imported.
# new_container_load targets are only available and tested on GitHub.
# BEGIN_DO_NOT_IMPORT
file_test(
    name = "new_pause_tar_test_index_json",
    content = """{
   "schemaVersion": 2,
   "manifests": [
      {
         "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
         "size": 586,
         "digest": "sha256:22e0bb47f6dd1938d7c30febe82ac37e99907fd8c76421ab87829ad6a3636bf9"
      }
   ]
}""",
    file = "@new_pause_tar//image-oci:index.json",
)

file_test(
    name = "new_pause_tar_test_oci_layout",
    content = """{
    "imageLayoutVersion": "1.0.0"
}""",
    file = "@new_pause_tar//image-oci:oci-layout",
)

file_test(
    name = "new_pause_tar_test_config_symlink",
    file = "@new_pause_tar//image:config.json",
    regexp = "{\"architecture\":\"amd64\",.*",
)

file_test(
    name = "new_pause_tar_test_0_tar_gz_symlink",
    file = "@new_pause_tar//image:0.tar.gz",
    regexp = ".*",
)

file_test(
    name = "new_pause_tar_test_1_tar_gz_symlink",
    file = "@new_pause_tar//image:1.tar.gz",
    regexp = ".*",
)
# END_DO_NOT_IMPORT
