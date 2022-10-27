E4ST Development
================

# Git Best Practices

## Workflow
Say I want to start working on a repository.  How would I get started? 
1. I would start off by finding (or creating) an [Issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues) to work on, which could correspond to a desired feature, a bug fix, etc.  I could find an issue by looking at a [Project](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects) (found in the Projects section at the top of this page) or at the Issues page of a github repository.
2. Next, I would select an issue that has a status of `Todo`, assign it to myself, then update the status to `In Progress`.
3. Then, I would create a new branch for the issue with a concise yet descriptive branch name.  Some guidelines on branch naming can be found [below](#branch-naming).
4. Then I'll develop the feature, make commits to the branch, and make sure to add tests, documentation and benchmarks where applicable.
5. Once I'm happy with the branch and am ready to merge it into the `main` branch, I'll create a [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) for the branch, linking the issue where applicable.  This will open up a forum for code review and run tests (where applicable).
6. From the pull request, the team will then iteratively review, suggest changes, etc.  Once the team signs off on the changes, the pull request should be approved, and that will merge the new branch into `master`.  The issue should also then be resolved.

## Making New Repositories
First thing you need to consider are whether a repository should be public or private.  Public packages are great for the modeling community, but we need to be mindful of sensitive information or program-specific development.  If the repository is supporting a specific work program for a sponsor, you should ask the sponsor if they would like for the modeling surrounding their program to be public or private.  Generally, it is better to err on the side of private, then to make public later on.

### Does my data belong in the repository?
It depends.  If it is a nominal, small example used for testing, this absolutely should live in the repository!  If it is a large data file that takes up several MB, please consider keeping that file locally, as the E4ST-Development group does have a storage capacity limit.

## Branch Naming
*  Prepend with a short token
   *  `wip` for work in progress - stuff I'm working on and know won't be finished for a while
   *  `feat` for a feature I'm adding or expanding
   *  `bug` for a bug fix
   *  `junk` for a throwaway branch created to experiment
   *  `doc` for a documentation branch 
*  Include issue number if applicable
*  Include a short description of the issue
*  Use dashes as a separator
*  Examples:
   *  `feat-14-load-inputs` - feature branch for issue 14 for loading inputs
   *  `bug-20-fix-opf` - bug branch for issue 20 for fixing the opf (hope this won't be a real branch :grimacing:)

# Julia Best Practices

## Style Guide
For consistency, let's try to follow the [Blue Style Guide](https://github.com/invenia/BlueStyle).

## Package Management
Julia has a rich ecosystem composed of registries.  Julia's [General Registry](https://github.com/JuliaRegistries/General) contains over 7,000 packages, all available with the super simple process of typing `add PackageName` in julia's package manager.  That will then download and install all the package dependencies, which can be many.  This is a beautiful thing, and we want to strive towards getting our packages onto this registry as well so that others can use them.  Please see [this link](https://julialang.org/contribute/developing_package/#how_to_develop_a_julia_package) for an overview of developing a julia package, including how to add additional dependencies.

## Continuous Integration
To facilitate higher software quality, it is good to establish a Continuous Integration (CI) pipeline, which is a set of things that are run when some condition is hit.  In the E4ST.jl repository, for example, the CI pipeline is run for Pull Requests.  It basically runs through all the tests in the package.  See [Julia's Unit Testing documentation](https://docs.julialang.org/en/v1/stdlib/Test/) for more information on unit testing.

### Testing
For testing, all files belong in the test folder.  The main file that is run is `test/runtests.jl`.  From there, the tests are broken up into test sets.  In order to run all the tests in the same way that would be run in the pipeline, you would run `]` to open the package manager, then type `test E4ST`.  This will freshly download all dependencies and run in a clean julia environment.  Since it has to download dependencies, it may take a while to run.  If you are developing tests and want to run them faster, you could run `include("test/runtests.jl")` directly to run the tests without having the extra runtime in creating a fresh julia environment.

### Benchmarking
For key functionality, we may wish to benchmark them to make sure that they are fast and efficient.  The benchmark framework compares the branch to the main branch.  for an example of the benchmark output, see [here](https://github.com/e4st-dev/E4ST.jl/pull/28#issuecomment-1282994388).  Benchmarks go in `benchmark/benchmarks.jl`, and should be added to the `SUITE` variable.
