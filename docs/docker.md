# Dockerfiles

Our test runners are deployed as Docker images.

- Your dockerfile should live at the root directory of your repository and it should be called `Dockerfile`.
- Your dockerfile should create the minimal image needed for your test runner to run the tests.
  Applying the official [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) can help to create a minimal image. The minimal-sized image is often based on Alpine Linux.
- The working directory should be `/opt/test-runner`.
- There should be a `/opt/test-runner/bin/run.sh` script that can be called with 3 parameters: the `exercise slug`, the path to the `solution folder`, and the path to the `output folder`.
  For more information see [The Interface](./interface.md).
- All changes to dockerfiles require a PR review from the @exercism/ops team.
- The docker image should include the [tooling-webserver](https://github.com/exercism/tooling-webserver/blob/master/README.md#installation-docker).

More background information and optional hints:

- To run the containers in production the `runc` command is used which does not use the `ENTRYPOINT` specified in the `Dockerfile`.
  It would still be good to have an `ENTRYPOINT` specified for local use and testing.
