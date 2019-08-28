# Rules in test runners

## Tests must be run in order.

Presuming we're un-skipping all tests, and want to continue to promote a TDD approach, we should ensure that the tests run in order so that the "next" test is the one that fails.stop as soon as the first test fails so that a user is only presented with one error message.

## The test runner must stop once a test fails.

Presuming we're un-skipping all tests, and want to continue to promote a TDD approach, we should stop as soon as the first test fails so that a user is only presented with one error message, and not overwhelmed. 

This also has the benefit of meaning test-running will happen quicker on solutions in an earlier stage of being developed (e.g. when a user is just trying to make the first test pass).
