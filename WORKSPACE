## Author : @adeeb10abbas
## Date : 2022-31-12
## Description : This file is used to build the workspace for the project
## LICENSE : MIT

workspace(name = "outdoorSLAM")

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository", "new_git_repository")
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

git_repository(
    name = "com_google_benchmark",
    branch = "main",
    remote = "https://github.com/google/benchmark",
)

git_repository(
    name = "com_google_googletest",
    branch = "main",
    remote = "https://github.com/google/googletest",
)

git_repository(
    name = "com_github_google_rules_install",
    branch = "main",
    remote = "https://github.com/google/bazel_rules_install",
)

load("@com_github_google_rules_install//:deps.bzl", "install_rules_dependencies")

install_rules_dependencies()

load("@com_github_google_rules_install//:setup.bzl", "install_rules_setup")

install_rules_setup()

all_content = """filegroup(name = "all", srcs = glob(["**"]), visibility = ["//visibility:public"])"""

new_git_repository(
    name = "opencv",
    branch = "4.x",
    build_file_content = all_content,
    remote = "https://github.com/opencv/opencv",
)

git_repository(
    name = "rules_foreign_cc",
    branch = "main",
    remote = "https://github.com/bazelbuild/rules_foreign_cc",
)

load("@rules_foreign_cc//foreign_cc:repositories.bzl", "rules_foreign_cc_dependencies")

rules_foreign_cc_dependencies()

############# DRAKE #############
DRAKE_TAG = "v1.10.0"
DRAKE_CHECKSUM = "78bd251bcfb349c988ee9225175a803a50cc53eaacdeb3bba200dfc82dcea305"  # noqa

http_archive(
    name = "drake",
    sha256 = DRAKE_CHECKSUM,
    strip_prefix = "drake-{}".format(DRAKE_TAG.lstrip("v")),
    urls = [
        "https://github.com/RobotLocomotion/drake/archive/refs/tags/{}.tar.gz".format(DRAKE_TAG),  # noqa
    ],
)

load("@drake//tools/workspace:default.bzl", "add_default_workspace")
load("@drake//tools/workspace:github.bzl", "github_archive")

add_default_workspace()
##########################

##### DRAKE ROS #####
## Adding Bazel_ROS2_Rules for drake-ros stuff to work ##
DRAKE_ROS_commit = "a4d976d6ef093031b4a08e31446527c814882501"
DRAKE_ROS_sha256 = "9c9cf7dc02cdcb562d10db6c17664692057d775ecf8d170d20f4b0f8a63ba137"
## Ref: ECousineau's awesome script - 
## https://github.com/EricCousineau-TRI/repro/blob/50c3f52c6b745f686bef9567568437dc609a7f91/bazel/bazel_hash_and_cache.py


github_archive(
        name = "bazel_ros2_rules",
        repository = "RobotLocomotion/drake-ros",
        extra_strip_prefix = "bazel_ros2_rules",
        commit = DRAKE_ROS_commit,
        sha256 = DRAKE_ROS_sha256,
        mirrors = {
            "github":["https://github.com/RobotLocomotion/drake-ros/archive/main.tar.gz",],
        }
    )

load("@bazel_ros2_rules//deps:defs.bzl", "add_bazel_ros2_rules_dependencies")
add_bazel_ros2_rules_dependencies()

github_archive(
    name = "drake_ros_core",
    repository = "RobotLocomotion/drake-ros",
    extra_strip_prefix = "drake_ros_core",
    commit = DRAKE_ROS_commit,
    sha256 = DRAKE_ROS_sha256,
    mirrors = {
        "github":["https://github.com/RobotLocomotion/drake-ros/archive/main.tar.gz",],
    }
)

github_archive(
    name = "drake_ros_tf2",
    repository = "RobotLocomotion/drake-ros",
    extra_strip_prefix = "drake_ros_tf2",
    # TODO(drake-ros#158): Use provided BUILD file.
    commit = DRAKE_ROS_commit,
    sha256 = DRAKE_ROS_sha256,
    mirrors = {
        "github":["https://github.com/RobotLocomotion/drake-ros/archive/main.tar.gz",],
    }
)

github_archive(
    name = "drake_ros_viz",
    repository = "RobotLocomotion/drake-ros",
    extra_strip_prefix = "drake_ros_viz",
    # TODO(drake-ros#158): Use provided BUILD file.
    commit = DRAKE_ROS_commit,
    sha256 = DRAKE_ROS_sha256,
    mirrors = {
        "github":["https://github.com/RobotLocomotion/drake-ros/archive/main.tar.gz",],
    }
)

load("@bazel_ros2_rules//deps:defs.bzl", "add_bazel_ros2_rules_dependencies")
add_bazel_ros2_rules_dependencies()

load("@bazel_ros2_rules//ros2:defs.bzl", "ros2_archive")
load("@bazel_ros2_rules//ros2:defs.bzl", "ros2_local_repository")

ROS2_PACKAGES = [
    "action_msgs",
    "builtin_interfaces",
    "console_bridge_vendor",
    "rclcpp",
    "rclcpp_action",
    "rclpy",
    "ros2cli",
    "ros2cli_common_extensions",
    "rosidl_default_generators",
    "tf2_ros",
    "tf2_ros_py",
    "visualization_msgs",
] + [
    # These are possible RMW implementations. Uncomment one and only one to
    # change implementations
    "rmw_cyclonedds_cpp",
    # "rmw_fastrtps_cpp",
]
# Use ROS 2
ros2_archive(
    name = "ros2",
    include_packages = ROS2_PACKAGES,
    sha256_url = "https://build.ros2.org/view/Hci/job/Hci__nightly-cyclonedds_ubuntu_jammy_amd64/lastSuccessfulBuild/artifact/ros2-humble-linux-jammy-amd64-ci-CHECKSUM",  # noqa
    strip_prefix = "ros2-linux",
    url = "https://build.ros2.org/view/Hci/job/Hci__nightly-cyclonedds_ubuntu_jammy_amd64/lastSuccessfulBuild/artifact/ros2-humble-linux-jammy-amd64-ci.tar.bz2",  # noqa
)

