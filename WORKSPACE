workspace(name = "bazel_repro")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# Add NodeJS rules (explicitly used for sass bundle rules)
http_archive(
  name = "build_bazel_rules_nodejs",
  sha256 = "4c702ffeeab2d24dd4101601b6d27cf582d2e0d4cdc3abefddd4834664669b6b",
  urls = ["https://github.com/bazelbuild/rules_nodejs/releases/download/0.28.0/rules_nodejs-0.28.0.tar.gz"],
)

load("@build_bazel_rules_nodejs//:defs.bzl", "check_bazel_version", "node_repositories", "yarn_install")


# The minimum bazel version to use with this repo is 0.18.0
check_bazel_version("0.18.0")

node_repositories(
  # For deterministic builds, specify explicit NodeJS and Yarn versions.
  node_version = "10.13.0",
  # Use latest yarn version to support integrity field (added in yarn 1.10)
  yarn_version = "1.12.1",
)

yarn_install(
  name = "npm",
  package_json = "//:package.json",
  # Ensure that the script is available when running `postinstall` in the Bazel sandbox.
  data = [
    "//:angular-tsconfig.json",
  ],
  yarn_lock = "//:yarn.lock",
)

# Install all bazel dependencies of the @ngdeps npm packages
load("@npm//:install_bazel_dependencies.bzl", "install_bazel_dependencies")
install_bazel_dependencies()

# Setup TypeScript Bazel workspace
load("@npm_bazel_typescript//:defs.bzl", "ts_setup_workspace")
ts_setup_workspace()

http_archive(
    name = "bazel_skylib",
    type = "tar.gz",
    url = "https://github.com/bazelbuild/bazel-skylib/releases/download/0.8.0/bazel-skylib.0.8.0.tar.gz",
    sha256 = "2ef429f5d7ce7111263289644d233707dba35e39696377ebab8b0bc701f7818e",
)

# Fetch transitive dependencies which are needed to use the karma rules.
load("@npm_bazel_karma//:package.bzl", "rules_karma_dependencies")
rules_karma_dependencies()