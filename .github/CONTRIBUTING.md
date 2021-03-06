# Contributing

We'd love your help! We want to make contributing to this project as easy and transparent as possible, whether it's:

- Reporting a bug
- Discussing the current state of the code
- Submitting a fix
- Proposing/add new features (like support for a new event source!)

## What to Contribute?!

Our [Issues Page](https://github.com/DataKind-DC/capital-nature-ingest/issues) has our backlog. If you find something that you'd like to work on, leave a comment to let us know! If you're a part of the DataKind GitHub org, you can even assign yourself an issue.

Most issues are requests for new event sources to be scraped. **If you want to work on a new event source**, you'll need to do the following:

- create a single `.py` file within the `events/` directory for your event source.
- make sure that your entire script can be run through the use of a single function named `main()`.
- add a key:value pair to the dictionary in `events/utils/event_source_map.py`; the key should be the name of the `.py` file you made and the corresponding value should be the Event Organizer name you've used in that `.py` file. 
- import your event source in `get_events.py` and add it to the `event_sources` list later on in the file. See lines 9 and 70, respectively. Please add alphabetically.
- write a test for your script
- lint your script with `flake8`

>You can find more details on each of these requirements by reading on.

## How to Contribute

We use [Github Flow](https://guides.github.com/introduction/flow/index.html), so all code changes happen through pull requests. We use a **Fork and Pull Model**, so go ahead and fork this repo if you haven't done so already.

### The Fork and Pull Model

#### Short Version

Before you start to work on something, fork this repo, clone the repository, and then make a new branch for your feature.

As you implement changes, commit them to your local branch, ideally following our [style guide](https://github.com/DataKind-DC/capital-nature-ingest/blob/master/.github/STYLE-GUIDE.md). When you're ready, push that branch to GitHub and then make a pull request. Then await review and, if necessary, push some more changes.

#### Long Version

##### Writing your Code

1. Fork the repository

Go to the project's [homepage](https://github.com/DataKind-DC/capital-nature-ingest) and hit the `Fork` button. This creates a copy of the repository in your GitHub account.

2. `clone` your forked repository

Now you want to clone the forked repo to your machine.

```bash
git clone https://github.com/your-user-name/capital-nature-ingest.git capital-nature-ingest-yourname
cd capital-nature-ingest-yourname
git remote add upstream https://github.com/DataKind-DC/capital-nature-ingest
```

This creates the directory *capital-nature-ingest-yourname* and connects your repository to the upstream (i.e. this) repository.

3. `checkout` a branch for your feature before writing any code

You want your master branch to reflect only production-ready code, so create a feature branch for making your changes. See our [Style Guide](https://github.com/DataKind-DC/capital-nature-ingest/blob/master/.github/STYLE-GUIDE.md) for naming your branch. To make the branch:

```bash
git checkout -b shiny-new-feature
```

This changes your working directory to the shiny-new-feature branch. Keep any changes in this branch specific to one bug/feature so it is clear what the branch brings to capital-nature-ingest.

Before creating this branch, make sure your master branch is up to date with the latest upstream master version. To update your local master branch, you can do:

```bash
git checkout master
git pull upstream master --ff-only
```

4. Do some work, see the changes, and stage files you've edited:

Once you’ve made changes, you can see them by typing:

```bash
git status
```

If you have created a new file, it is not being tracked by git. Add it by typing:

```bash
git add path/to/file-to-be-added.py
```

Doing ‘git status’ again should give something like:

```bash
# On branch shiny-new-feature
#
#       modified:   /relative/path/to/file-you-added.py
#
```

5. `commit` your changes to your local branch

Once you've added files, you're ready to commit your changes to your local repository with an explanatory message. Use the `-m` flag if you don't want to include a full commit body. See the [style guide](https://github.com/DataKind-DC/capital-nature-ingest/blob/master/.github/STYLE-GUIDE.md) for details on how to format your commit messages.

```bash
git commit -m "ENH: add scraper for awesome new source (#123)"
```

>Note how the commit message includes an informative prefix as well as the relevant issue number

6. When you want your changes to appear publicly on your GitHub page, push your forked feature-branch’s commits

```bash
git push origin shiny-new-feature
```

Above, *origin* is the default name given to your remote repository on GitHub. This will create a shiny-new-feature branch in your origin repository, ie. your repository on GitHub.

You can see the remote repositories:

```bash
git remote -v
```

If you added the upstream repository as described above you will see something like:

```bash
origin  git@github.com:yourname/capital-nature-ingest.git (fetch)
origin  git@github.com:yourname/capital-nature-ingest.git (push)
upstream        git://github.com/capital-nature-ingest.git (fetch)
upstream        git://github.com/capital-nature-ingest.git (push)
```

Once you've pushed your code, your code is on your GitHub repo and is not yet a part of the main project. For that to happen, a **Pull Request (PR)** needs to be submitted on GitHub.

But first, let's lint and test your code.

#### Lint your Code

We try to follow subset of the [PEP8](https://www.python.org/dev/peps/pep-0008/) standard by using [Flake8](http://flake8.pycqa.org/en/latest/) to ensure a consistent code format throughout the project.

Although Continuous Integration will run Flake8 and report any stylistic errors in your code, it is helpful before submitting code to run the check yourself:

```bash
flake8 <your-file>.py
```

The output will tell you the line and column numbers where there are stylistic errors. It'll also tell you the related PEP8 standard.

>Optionally, you may wish to setup [pre-commit](https://pre-commit.com/) hooks to automatically run flake8 when you make a git commit.

#### Testing your Code

Before you initiate a pull request, you need to add a test suite that ensures you're code properly schematizes each scraped event's data fields based on our [event schema](https://github.com/DataKind-DC/capital-nature-ingest/blob/master/event_schema.md).

We've tried to make this as easy as a copy-paste, so here's what to do.

1. Navigate to the `tests/` directory within this repo and make a new file that is the name of your event scraper file plus a `_test.py` suffix:

bash
```
cd tests/
touch <name_of_your_scraper>_test.py
```

2. Open this file you just made and copy-paste the contents of any other test file (e.g. [this one](https://github.com/DataKind-DC/capital-nature-ingest/edit/master/tests/ans_test.py)) into your new test file.

3. Change the import statement on line 5 to reference you're event scraper file

```python
...
from events.ans import main # change this line to import events.<your_file>
...
```

4. To verify that your scraper can pass the tests, `cd` to the root of the repo, make sure you've activate the virtual environment, and then run the test:

```bash
cd path/to/root/of/this/repo #if you're not already here
source env/bin/activate #if you haven't already done this
python3 -W ignore -m unittest tests/<name_of_your_scraper>_test.py
```

5. If your tests pass, you'll see `OK` logged to your terminal. If not, inspect the errors that were raised and update your code accordingly. If you've got questions on how to interpret the test output, feel free to [create an issue](https://github.com/DataKind-DC/capital-nature-ingest/issues) or contact us on Slack.

#### Submitting a Pull Request

So you've written some code, added your tests, and passed your tests. Now you're ready to ask for a code review! Let's initiate a Pull Request. But first...

##### One Final Review

Before you start a pull request, let's double check your branch changes against the branch it was based on:

1. Navigate to your repository on GitHub.

2. Click on `Branches`.

3. Click on the `Compare` button for your feature branch.

4. Select the base and compare branches, if necessary. The defauly is usually right and this will pretty much always be `master` and your `shiny-new-feature` branch, respectively.

So what are we doing? Here you're checking to make sure your feature branch doesn't conflict with the base branch. It shouldn't, since the files you added never existed on the base branch. But it can happen and this little step can save you some trouble.

##### Initiate a Pull Request

Now you're ready to initiate a pull request. A pull request is how code from a local repository becomes available to the GitHub community and can be looked at and eventually merged into the master version.

To submit a pull request:

1. Navigate to your repository on GitHub
2. Click on the `Pull Request` button
3. You can then click on `Commits` and `Files Changed` to make sure everything looks okay one last time
4. Write a description of your changes in the `Preview Discussion` tab
5. Click `Send Pull Request`.

This request then goes to the repository maintainers, and they will review the code.

#### Updating a Pull Request

Sometimes, you'll need to update your pull request.

If there's a gray bar saying "This pull request cannot be automatically merged.", then you've got some updates to make. That's because the file(s) that your pull request modifies was/were updated in the meantime. To avoid a conflict on the remote, GitHub won't let you automatically merge into master. This means you need to update your Pull Request by merging the upstream's master branch into your feature branch. In short, you need to “merge upstream master” in your branch:

```bash
git checkout shiny-new-feature
git fetch origin
git merge origin/master
```

If there are no conflicts (or if they could be fixed automatically), a file with a default commit message will open, and you can simply save and quit this file.

If there are merge conflicts, you'll need to solve those conflicts. See [this](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/) for an explanation on how to do this. Once the conflicts are fixed and merged and the files where the conflicts were solved are added, you can run `git commit` to save those fixes.

>If you have uncommitted changes at the moment you want to update the branch with master, you will need to stash them prior to updating (see the [stash docs](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)). This will effectively store your changes and they can be reapplied after updating.

After your feature branch has been updated locally, you can now update your pull request by pushing to the branch on GitHub:

```bash
git push origin shiny-new-feature
```

#### Deleting your Branch (optional)

Once your pull request is accepted, you’ll probably want to get rid of the feature branch. You can do that to the remote master right within the pull request page. To delete the branch locally, you need to first pull the remote master down into your local master. Then git will know it's safe to delete your branch.

```bash
git checkout master
git fetch origin
git merge origin
```

Now that your local master is even with the remote, you can do:

```bash
git branch -d shiny-new-feature
```

> Make sure you use a lower-case -d, or else git won’t warn you if your feature branch has not actually been merged.

#### Syncing up with the Upstream Master

If you want to continue to collaborate on the project, you'll need to make sure your forked repo is current with the official one at `https://github.com/DataKind-DC/capital-nature-ingest`. To do so, you need to "merge upstream master". First, checkout your master branch

```bash
git checkout master
```

Then use `git status` to make sure you don't have uncommitted changes at the moment you want to update the branch with the upstream master. If you do, which you shouldn't, you will need to [stash](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning) them prior to updating. 

If all's good, merge the upstream master:

```bash
git pull upstream master --ff-only
```

Read [this](https://docs.gitlab.com/ee/user/project/merge_requests/fast_forward_merge.html) to learn about the `--ff-only` argument.

Another way of doing the above is:

```bash
git checkout master
git fetch upstream
git merge upstream/master  #git pull upstream/master could have replaced the last two lines
```

## Reporting bugs or making feature requests

We use GitHub issues to track bugs and make feature requests. [Open a new issue](https://github.com/DataKind-DC/capital-nature-ingest/issues) if you've got an idea. An issue template will autopopulate. Feel free to use only those sections of the issue template that you think are relevant.

**Great Bug Reports** tend to have:

- A quick summary and/or background
- Steps to reproduce
  - Be specific!
  - Give sample code if you can.
- What you expected would happen
- What actually happens
- Notes (possibly including why you think this might be happening, or stuff you tried that didn't work)

People *love* thorough bug reports. I'm not even kidding.

## License

[here](https://github.com/DataKind-DC/capital-nature-ingest/blob/master/.github/LICENSE)
