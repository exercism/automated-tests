# The Interface

All interactions with the Exercism website are handled automatically. Test runners have the single responsibility of taking a solution, running its tests and returning a standarised output.

## Execution

- An test runner should provide an executable script. You can find more information in the [docker.md](https://github.com/exercism/automated-mentoring-support/blob/master/docs/docker.md) file.
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
      "name": Test that the thing works",
      "status": "pass", # pass, fail, error, skip
      "expected": "42",
      "actual": "123123",
      "message": nil
    }
  ]
}
```

The following statuses are valid:
- `pass`: All tests passed
- `failed`: All tests failed
- `error`: To be used when the tests did not run correctly (including if there are any skips)

## Debugging

The contents of `stdout` and `stderr` from each run will be persisted into files that can be viewed later.

You may write an `report.out` file that contains debugging information you want to later view.

At a later date, we will provide an interface for you to download these files along with the submitted files and `report.json`.
