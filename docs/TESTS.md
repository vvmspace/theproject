# Tests

## E2E API Tests

We cover our API and business logic with E2E API tests as the source of truth that feature / flow / whole project works as expected.

It saves us time, improves development experience and gives us flexibility to change implementation details without breaking the whole project.

## Unit Tests

Unit tests could be used for testing specific important parts of code in isolation, when E2E API tests are not enough to cover the logic.


## Tests Environment

See [ENVIRONMENTS.md](ENVIRONMENTS.md) and [LOCAL_TO_PRODUCTION_PIPELINE.md](LOCAL_TO_PRODUCTION_PIPELINE.md) for context.

Tests as a part of CI/CD pipeline should be runnable in isolated `test` environment after running migrations and seeding using single command and be fully automated.

## Integration testing

- Dump the next stage database or a piece of it.
- Restore it in `test` environment.
- Run `test` environments with tests.
- Removes environment after tests.
- On failure block merge to the next stage.

## Mocking

When we run tests in `local` or `test` environments we use fixed prefixes.

Example: GET `/api/v1/auth/google/callback?code=google_mock_code_1&...` returns JWTAuthToken for user with id 1.

