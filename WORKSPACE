# Cannot have the '.bazel' extension or the docker rules fail.

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

##########
# Python
##########
http_archive(
    name = "rules_python",
    url = "https://github.com/bazelbuild/rules_python/releases/download/0.2.0/rules_python-0.2.0.tar.gz",
    sha256 = "778197e26c5fbeb07ac2a2c5ae405b30f6cb7ad1f5510ea6fdac03bded96cc6f",
)
load("@rules_python//python:pip.bzl", "pip_install")
pip_install(
   name = "pip_requirements",
   requirements = "//third_party:requirements.txt",
)

##########
# Docker
##########
http_archive(
    # Get copy paste instructions for the http_archive attributes from the
    # release notes at https://github.com/bazelbuild/rules_docker/releases
    name = "io_bazel_rules_docker",
    sha256 = "59d5b42ac315e7eadffa944e86e90c2990110a1c8075f1cd145f487e999d22b3",
    strip_prefix = "rules_docker-0.17.0",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.17.0/rules_docker-v0.17.0.tar.gz"],
)
load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)
container_repositories()

load("@io_bazel_rules_docker//repositories:deps.bzl", container_deps = "deps")
container_deps()

load("@io_bazel_rules_docker//python:image.bzl", _py_image_repos = "repositories")
_py_image_repos()

load("@io_bazel_rules_docker//container:container.bzl", "container_pull")

container_pull(
    name="python3.8-base",
    registry="index.docker.io",
    repository="python",
    tag="3.8.10-slim-buster"
)

container_pull(
    name="python3.9-base",
    registry="index.docker.io",
    repository="python",
    tag="3.9-slim-buster"
)
