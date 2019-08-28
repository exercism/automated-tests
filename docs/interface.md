# The Interface

All interactions with the Exercism website are handled automatically. Test runners have the single responsibility of taking a solution, running its tests and returning a standardised output.

## Execution

- A test runner should provide an executable script. You can find more information in the [docker.md](https://github.com/exercism/automated-mentoring-support/blob/master/docs/docker.md) file.
- The script will receive two parameters:
  - The slug of the exercise (e.g. `two-fer`).
  - A path to a directory containing the submitted file(s) (with a trailing slash).
- The script must write a file to that directory named `report.json`

## Output format

The `report.json` file should be structured as followed:

```json
{
  "status": "...",
  "tests": [
    {
      "name": "Test that the thing works",
      "status": "pass",
      "expected": "42",
      "actual": "123123",
      "message": "Expected 42 but got 123123"
    }
  ]
}
```

The following overall statuses are valid:
- `pass`: All tests passed
- `fail`: All tests failed
- `error`: To be used when the tests did not run correctly (including if there are any skips)


The following per-test statuses are valid:
- `pass`: The test passed
- `fail`: The test failed failed
- `error`: The test errored
- `skip`: The test was skipped

Where possible, `expected` should show the output that was expected and `acutal` should show what was actually given.

The `message` can be used to display human-readable error messages. Presume that whatever is written here will be displayed to the student.

**We will provide a Ruby script that converts JUnit to this JSON output, which you can add as a final step of your Docker images, if you prefer to use an existing format.**

## Debugging

The contents of `stdout` and `stderr` from each run will be persisted into files that can be viewed later.

You may write an `report.out` file that contains debugging information you want to later view.

At a later date, we will provide an interface for you to download these files along with the submitted files and `report.json`.
