# bazel-test
Repository for testing out https://bazel.build

# Instructions
1. Install bazel. Follow [this](https://docs.bazel.build/versions/4.1.0/bazel-overview.html#installing-bazel).
2. Build the project.
   * To build the entire project, run `bazel build //...`
   * To build a specific package, run `bazel build //<package>`. For instance, `bazel build //microservice1` 
3. Load the built Docker images:
   * Load locally: run `bazel run //base-images:python3.8`. Also, running a `bazel build` after making changes 
     essentially does nothing. Must run the `bazel run` to see any new changes. After this you can run
     `docker run` on the image.
   * Load to remote repo: run `bazel run //base-images:python3.8-push` (this will fail due to auth error)

# Project Layout
* base-images - Multiple rules to build docker images. The images defined therein would be customized
  base images used to containerize the microservices. 
* library1 - Basic bazel 'library'. No external dependencies.
* microservice1 - Basic, standalone microservice. It has no external dependencies.
* microservice2 - Microservice which depends on library1 and nothing else
* microservice3 - Microservice which depends on a PyPi library (numpy). Building 
  the Docker image is fragile. If the host Python version doesn't match the 
  version in the container then you'll get an error. See [this](https://github.com/bazelbuild/rules_docker/issues/1314).  
* wheel1 - Example showing how to build a wheel. The python rule set doesn't appear
  to be able to publish it. Nor does it appear as though you can use the wheel in
  the project. Maybe look at https://github.com/vaticle/bazel-distribution

# Notes
* When using Bazel, the assumption is you use Bazel for pretty much all operations (building, running, etc.).
* Building Docker images does not require the use of the Docker daemon, or any part of Docker. Rather, a .tar suitable
  for `docker load` is produced. As a result, there are no Dockerfiles at all in this project.
* Python libraries from PyPi are (sort of) global. You can apparently have multiple requirements.txt
  files, but each one must have its own rule in the WORKSPACE file. The scalability
  of this effectively means global versions are going to be used.

# Other Stuff
Running bazel reference: https://docs.bazel.build/versions/main/guide.html  
Python rules: https://github.com/bazelbuild/rules_python
Docker rules: https://github.com/bazelbuild/rules_docker