# Mobster test action

Tests the screens a pull request changed, on simulators or your own iPhone, and keeps one comment on the pull request with the verdict, the frames and GIFs. It runs [`mobster test pr`](https://docs.mobster.dev/testing#test-a-pull-request) on a macOS runner with [Mobster for Mac](https://mobster.dev/download) installed, through the agent inside the app.

```yaml
name: mobster
on: pull_request
permissions:
  contents: read
  pull-requests: write
jobs:
  test:
    runs-on: [self-hosted, macOS]   # a Mac with Mobster for Mac in Applications
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: RadishSoftware/test-action@v1
        with:
          scheme: YourApp
```

Every input, the comment it posts, and how to run it on your iPhone: [docs.mobster.dev/ci](https://docs.mobster.dev/ci#the-mobster-test-action).

A phone (`device`) or a TestFlight app (`testflight`) named in the workflow runs only when the runner's `.env` lists it in `MOBSTER_RUNNER_DEVICES` or `MOBSTER_RUNNER_TESTFLIGHT`, because a pull request can edit the workflow file but not the runner.

Use the `device` and `testflight` inputs for phones. The action refuses `--device` and `--testflight` in `args`.

On the Team plan, set `MOBSTER_TEAM_KEY` (an upload key from the console, as a secret) in the step's `env`: the action adds `--share`, and the comment links your team's hosted report instead of the artifact ([docs.mobster.dev/ci](https://docs.mobster.dev/ci#share-the-report-with-your-team)).

This folder is the action's source. It is published as `RadishSoftware/test-action`. `action.py` uses the standard library only and runs on the runner's own `python3` (3.9 or later); its tests are `mobile_agent/tests/test_test_action.py`.
