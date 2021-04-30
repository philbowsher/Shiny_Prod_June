# `shinytest` Example

An example application (OpenFDA Adverse Events Data) for
using and illustrating how `shinytest` works.

## How to use this repository

- Check it out locally to see how `shinytest` works
- Run the tests interactively yourself
- Make "breaking changes" to the application and see the tests fail
- Create a repository on GitHub (it cannot be a fork, or your actions will not run)
- Change the GitHub actions as discussed below
    - Change the Connect URL to your own Connect URL
    - Add your own `CONNECT_API_KEY` secret
- Send a PR to _your own_ `main` branch (within your repository)
- Use or improve this pattern for your own work!!

## Run `shinytest`

In order to run `shinytest`, you need to:

- Activate the renv environment: `renv::activate()`
- Install dependencies with `renv::restore()`
- Run tests with `shinytest::testApp()`
- If there are differences, view the differences with
`shinytest::viewTestDiff(testnames = "mytest")` (for the `mytest` test)

## CI Deployment to RStudio Connect

### Using git-backed deployment

This requires a `manifest.json` in the repository. It is created with
`rsconnect::writeManifest()`

RStudio Connect supports a "pull-based" git-backed deployment and polling.

In this mode, Connect will watch a branch of your repository and deploy any new
commits that it detects, depending upon a server-wide polling interval.

This is sufficient for most workflows, and is simpler to set up successfully.

[You can read more about the process here](https://docs.rstudio.com/connect/user/git-backed/)


### Using GitHub Actions

This is a more advanced and more flexible workflow but is more complex and
tricky to set up properly.

> If you create your own `manifest.json`, you may need to remove your `.Rprofile`
> before generating it, or edit the `manifest.json` and remove the `.Rprofile`
> record from "files".
> 
> This will be improved in a future version of the GitHub Action

- The approach we take presumes a `manifest.json`. This _could_ be generated by
a GitHub action, but instead we generate during our dev work using
`rsconnect::writeManifest()`

- Then create a
[`.github/workflows/rstudio-connect.yaml`](./.github/workflows/rstudio-connect.yaml)
file

- Specify your `url: ` (should be your RStudio Connect URL)

- In GitHub Actions on your repo, set the `CONNECT_API_KEY` secret
   - Go to the repository on GitHub
   - Navigate to Settings > Secrets
   - Create a "New Repository Secret" with an API key

- Make changes to the application and see the automatic deployment work!

## CI testing with `shinytest` and GitHub Actions

- Create a
[`.github/workflows/run-shinytest.yaml`](./.github/workflows/run-shinytest.yaml)
file
- If the tests succeed, the workflow run will pass
- If the tests differ or fail, the workflow run will fail
- To review the results directly, you have to download the build artifacts
- Alternatively, since tests are failing, it means that something about the
application has changed. Tests may need to be updated (or code fixed) to address
the changes. To do this, you can run the tests locally or address any code
changes necessary

## Future Exploration (for users)

Some other areas you can explore for your own apps / assets:

- Implement CI workflows for your projects where it makes sense (don't automate until you need to!)
- Only deploy to Connect if tests complete successfully
- Change configuration of what tests run where / etc.

## Future Development Work (for developers)

Our goal is to continue improving this process and making it easier! Some
current ideas of next steps. Your feedback, contribution, and ideas are welcomed
and encouraged!

- Make sure all links / etc. create a usable workflow
- Make GitHub actions easier to extend, add functionality (i.e. content tags on Connect, etc.)
- Add `shinytest` example actions to `usethis` to follow the
`usethis::use_github_action()` pattern
- Make example `shinytest` actions that integrate more effectively with RStudio
Connect for reviewing results, etc.
- Fix the Windows shinytest running (currently fails)
- Make system deps (on Ubuntu) run more effectively / dynamically ([r-lib
patterns](https://github.com/r-lib/actions) orient mostly around packages)
- Package up more of these items as re-usable components to simplify the user workflows
- Better patterns around using / accepting test changes, artifacts, etc.
- Figure out cross-operating-system compatibility issues
- Make renv easier to use in GHA
