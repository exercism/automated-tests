# Dockerfiles

Our test runners are deployed as docker images.

- Your dockerfile should live at the root directory of your repository and it should be called `Dockerfile`
- Your dockerfile should be the minimal needed for your test runners and create the minimal image needed for running the tests.
  It should use alpine linux if possible.
- The working directory should be `/opt/test-runner`
- There should be a `/opt/test-runner/bin/run.sh` script that can be called with 3 parameters: the `exercise slug`, the path to the `solution folder`, and the path to the `output folder`.
  For more information see [The Interface](./interface.md).
- All changes to dockerfiles require a PR review from the @exercism/ops team.

More background information and optional hints:
- To run the containers in production the `runc` command is used which does not use the `ENTRYPOINT` specified in the `Dockerfile`.
  It would still be good to have an `ENTRYPOINT` specified for local use and testing.
- The students solution is currently mounted to `/mnt/exercism-iteration/` in production.
  We still pass in the folder as a second argument to the analyzer in case that changes in the future.
