# The Interface

All interactions with the Exercism website are handled automatically. Test runners have the single responsibility of taking a solution, running all tests and returning a standardised output.

## Execution

- A test runner should provide an executable script. You can find more information in the [docker.md](./docker.md) file.
- The script will receive three parameters:
  - The slug of the exercise (e.g. `two-fer`).
  - A path to an input directory (with a trailing slash) containing the submitted solution file(s) and any other exercise file(s). This directory should be considered as read-only. As [@ccare specified](https://github.com/exercism/automated-tests/issues/38#issuecomment-576335584) it's technically possible to write into it but it's better to use `/tmp` for temporary files (e.g. for compiling sources).
  - A path to an output directory (with a trailing slash). This directory is writable.
- The script must write a file to the output directory named `results.json`
- The runner must exit with an exit code of 0 if it has run successfully, regardless of the status of the tests.

## Output format

The `results.json` file should be structured as followed:

```json
{
  "status": "fail",
  "message": null,
  "tests": [
    {
      "name": "Test that the thing works",
      "status": "fail",
      "message": "Expected 42 but got 123123",
      "output": "Debugging information output by the user"
    }
  ]
}
```

### Top level

The following overall statuses are valid:
- `pass`: All tests passed
- `fail`: At least one test failed
- `error`: To be used when the tests did not run correctly (e.g. a compile error, a syntax error)

#### Message

Where the status is `error` (the tests fail to execute cleanly), the top level `message` key should be provided. It should provide the occurring error to the user. As it is the only piece of information a user will receive on how to debug their issue, it must be as clear as possible.. For example, in Ruby, in the case of a syntax error, we provide the error and stack trace. In compiled languages, the compilation error should be provided.

When the status is not `error`, either set the value to `null` or omit the key entirely.

### Per-test

#### Statuses

The following per-test statuses are valid:
- `pass`: The test passed
- `fail`: The test failed
- `error`: The test errored

#### Messages

The per-test `message` key can be used to display human-readable error messages. Presume that whatever is written here will be displayed to the student. If there is no error message, either set the value to `null` or omit the key entirely.

#### Output

The per-test `output` key should be used to store and output anything that a user deliberately outputs for a test.

- It should be attached to all test results that produce user output.
- Only content outputted by a user manually should show - not automatic output by the test-runner.
- You may either capture content that is output through normal means (e.g. `puts` in Ruby, `print` in Python or `Debug.WriteLine` in C#), or you may provide a method that the user may use (e.g. the Ruby Test Runner provides a user with a globally available `debug` method that they can use, which has the same characteristics as the standard `puts` method).
- The output **must** be limited to 500 chars. Either truncating with a message of "Output was truncated. Please limit to 500 chars" or returning an error in this situation are acceptible.

**We will provide a Ruby script that converts JUnit to this JSON output, which you can add as a final step of your Docker images, if you prefer to use an existing format.**

## Debugging

The contents of `stdout` and `stderr` from each run will be stored in files that can be viewed later.

You may write an `results.out` file to the output directory, which contains debugging information you want to later view.

At a later date, we will provide an interface for you to download these files along with the submitted files and `results.json`.
