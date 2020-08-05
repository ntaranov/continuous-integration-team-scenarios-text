# Common Continuous Integration Team Scenarios

You learned Git commands but want to know how real-world Continuous Integration is done? Or maybe you already doing it but want to fine-tune your everyday actions? This course will give you practical skills of doing Continuous Integration (CI) in a GitHub-based repository. The course isn't intended to be a click-through wizard-style experience, instead you'll do what developers do during their daily work the way they do it. I'll explain the theory as you reach relevant steps.

## 💻 What are we going to do?

As we progress, we will incrementally build a memo with a common CI steps sequence, which is a great way to memorize it. In other words, we will make  the list of actions which developers take doing CI doing CI. We'll also use some simple test suite to mimic actual CI process.  

Here is a GIF showing commits in your repository as you will go through the course. As you see, nothing here is complicated and everything is essential.

![Continuous Integration Steps](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/flow.gif)

You'll go through these common CI scenarios:
- Working on a feature;
- Using auto tests to ensure quality;
- Implementing a bugfix;
- Resolving merge conflicts;
- Dealing with a bug in production.

## 📖 What you'll learn

<!-- TODO convert to links -->
You'll be able to answer the questions:
- What is Continuous Integration, really?
- What types of auto tests are used with CI and when they are triggered?
- What are pull requests and when you need them?
- What is Test Driven Development and how it relates to CI?
- To merge or to rebase?
- To roll back or to fix forward?

## ❔ What is Continuous Integration, really?

**Continuous integration**, or CI, is a technical practice when each member of a team integrates his code at least once a day so that the resulting code at least builds without errors.

<details><summary>There are various interpretations of the term.</summary>
One point of disagreement is the frequency of integration. Some argue that integrating just once a day is not enough to actually integrate continuously. A counterexample is when everybody gets fresh code once in the morning and integrates once in the evening. Though the argument stands, it's mostly considered that "once a day" definition is practical enough, it's concrete and is good for teams of different sizes.  
  
Another point is that C++ in not the only language used in development for a long time, and just building without errors is a very weak validation requirement. Some set of tests (e.g. unit tests run on a local computer) should be required to pass. Currently, community seems to gravitate towards such requirement, and in the future "build + units tests" will likely be the mainstream, if not already.
</details>

Continuous integration is different from continuous deployment or continuous delivery (CD) in that we are not required to have a release candidate after each integration cycle.  

### This is the list of CI steps we will use during the course

1. Pull in the latest code. Create a branch from `master`. Start working.  
2. Create commits on your new branch. Build and test locally. Pass? Go to the next step. Fail? Fix errors or tests and try again.  
3. Push to your remote repository or remote branch.  
4. Create a pull request. Discuss the changes, add more commits as discussion continues. Make tests pass on the feature branch.  
5. Merge/rebase commits from master. Make tests pass on the merge result.  
6. Deploy from the feature branch to production.
7. If everything is good in production for some period of time, merge changes to master. 

![Continuous Integration process (version)](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/ci.png)

## ⚙️ Before you begin

### Have the software in place

To complete the course you will need [Node.js](https://nodejs.org/) and a [Git client](https://git-scm.com/downloads) installed.
> 👉 You can use whatever Git client you prefer, but I will provide reference solutions for command line only.

<details><summary>Make sure, you have a command line client. Click here for more details.</summary>

If you don't have a command line client yet, you can find the installation instructions [here](https://git-scm.com/downloads).

> 👉 By the way, I will use such collapsible sections like this to provide extra information during the course.
</details>

### Setup the repository

You will need to fork [the course starter repository](https://github.com/ntaranov/continuous-integration-team-scenarios-students) on GitHub. Let's agree to call your fork of the repository __course repository__.

Done?
If you went with the default configuration, you will likely now have a repository named `continuous-integration-team-scenarios-students` under your GitHub account, and the URL probably looks like 
```
https://github.com/<your GitHub user name>/continuous-integration-team-scenarios-students
```
I'll just refer to it as `<repository URL>` from now on.
> 👉  I will use angle brackets like `<this>` when you need to substitute the expressions with whatever values relevant for you. 

Make sure that **GitHub actions** are enabled for the course repository. If actions are not enabled, please enable them clicking the button in middle of the screen under the Actions GitHub UI tab. 
> 👉 You will not be able follow the instructions in the course if GitHub Actions are not enabled.

![Enable Github Actions for the repository](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/actions.png)

You can always use GitHub's ability to render Markdown to see the current state of the list we compose at
```
https://github.com/<your GitHub user name>/continuous-integration-team-scenarios-students/blob/master/ci.md
``` 


### About solutions

Even though the best way to complete this course is doing everything by your own hands, you potentially might find yourself in trouble. 

If you feel that you don't understand what to do and cannot continue, you can refer to `solution` branch included in your starting repository. Please, don't merge `solution` into `master` during the course. You can use the branch to figure out what you need to do, or to compare your code with the reference implementation using all the capabilities Git provides. If you feel completely lost, you can replace your `master` branch commits with `solution` commits entirely and then reset your working directory to whatever step of the course you desire. 

<details><summary>Use it only if you really need to.</summary>
Commit your current work before doing this.

```bash
git add .
git commit -m "Backing up my work"
```

The following commands

- rename master as master-backup;  
- rename solution as master;  
- check out the new master and overwrite the working directory;  
- create branch "solution" from "master" (being "solution" in past) just is in case if need "solution" in future.  

```bash
git branch -m master master-backup
git branch -m solution master
git checkout master -f
git branch solution
```

After performing these actions, you can use `git log master` to figure out what commit you need.
Then you can reset your working directory to it using 
```bash
git reset --hard <the SHA you need>
```

If you are happy with the result, eventually you might want to push the remote. Don't forget to update the remote branch accordingly when you push. 

```bash
git push --force origin master
```

Please, note that we use `git push --force`. You will rarely ever want to force push to a remote's master, but here we have very specific scenario with only one collaborator who knows what he is doing.
</details>


## 🏁 Starting working

![Continuous Integration: Starting working](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/1.gif)
  

Let's start composing our CI steps list. You would commonly start this step with pulling the latest code from the remote repository, but we don't have a local repository set up yet, so we clone it instead.  

### ⌨️ Activity: Pull in the latest code, create a branch from `master`, start working

1. Clone the course repository from `<repository URL>`.  
2. Run `npm install` in the course repository's directory; we need it to install Jest which we use to run tests.
3. Create a branch and name it `feature`. Check out the branch.  
4. Add the tests code to `ci.test.js` between the comments asking to do so.
  ```js
    it('1. pull latest code', () => {
      expect(/.*pull.*/ig.test(fileContents)).toBe(true);
    });

    it('2. add commits', () => {
      expect(/.*commit.*/ig.test(fileContents)).toBe(true);
    });

    it('3. push to the remote branch with the same name', () => {
      expect(/.*push.*/ig.test(fileContents)).toBe(true);
    });

    it('4. create a pull request and continue working', () => {
      expect(/.*pull\s+request.*/ig.test(fileContents)).toBe(true);
    });
  ```
5. Add the text with the first 4 steps to "ci.md" file.
  ```
  1. Pull in the latest code. Create a branch from `master`. Start working.    
  2. Create commits on your new branch. Build and test locally.  
    Pass? Go to the next step. Fail? Fix errors or tests and try again.  
  3. Push to your remote repository or remote branch.  
  4. Create a pull request. Discuss the changes, add more commits  
    as discussion continues. Make tests pass on the feature branch.  
  ```

<details><summary>Show the commands...</summary>

```bash
# Clone the course repository
git clone <repository URL>
cd <repository name>

# Run npm install in the course repository's directory; we need it to install Jest which we use to run tests.
npm install

# Create a branch and name it feature. Check out the branch.
git checkout -b feature

# Edit ci.test.js as described above
# Edit ci.md as described above
```

</details>


## 🔨 Create commits on your new branch, build and test locally

We are going to set up tests to run on commit and then commit the code.  

### Scenarios when tests are triggered automatically

- Locally:
  - Constantly or in response to relevant code changes with immediate feedback;
  - On save (more often for interpreted or JIT compiled languages);
  - On build (more often when compilation is required);
  - On commit;
  - On push.

- On the build server or build environment:
  - When code is pushed to a private branch / repository. 
    - The branch code is tested.
    - The potential merge result (usually with `master`) is tested. 
  - In a stage of Continuous Integration / Continuous Delivery pipeline

Generally, the less time it takes to run a test suite, the more often you can afford to run it. Common arrangement might look like this.

- Fast unit tests - on build, in CI pipeline
- Slow unit tests, fast component and integration tests - on commit, in CI pipeline
- Slow component and integration tests - in CI pipeline
- Security, capacity and other lengthy or expensive tests - in CI/CD pipelines, but only on certain build modes/stages/pipelines, e.g., when preparing a release candidate or when launched manually.

### ⌨️ Activity

I suggest first running tests manually using `npm test` command. After, let's add a git hook to run our tests on commit. There is a catch, Git hooks are not considered a part of repository and thus cannot be cloned from GitHub with the rest of the course materials. To install the hook, you'll need to run `install_hook.sh` or copy file `repo/hooks/pre-commit` into your local `.git/hooks/` directory.
When you commit, you'll see test running and checking if certain keywords are present in the list.

1. Run tests manually executing `npm test` command in your course repository folder. Make sure test run.  
1. Install the pre-commit hook executing `install_hook.sh`.  
1. Commit changes to local repository.  
1. Make sure the tests run before the commit.  

You repository should look like this after the actions.  
![Continuous Integration: First commit](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/2.gif)
  


<details><summary>Show the commands...</summary>

```bash
# Install the pre-commit hook executing install_hook.sh.  

# Commit changes to local repository. Use "Add first CI steps" as the commit message.
git add ci.md ci.test.js
git commit -m "Add first CI steps"

# Make sure the tests run before the commit.  
```
</details>


## ⏫ Push to your remote repository or remote branch

After doing work locally, developers usually make their work publicly available so that it could be eventually merged into a shared codebase. Using GitHub, it's either done publishing the work to a personal fork or a branch.

- When using forks, a developer forks a remote shared repository creating a personal remote copy of it also known as fork. After, he clones the personal repo to work with it locally. When work is done and the commits are created, he pushes them to his fork where they are available to others and can be merged into the shared repository. This approach commonly used when contributing to open source projects on GitHub. It is also used in my extended [Team Work and CI with Git](https://devops.redpill.solutions/) course.
- Another approach is to use single remote and to consider only shared remote's `master` branch "protected". In this scenario individual developers push their work into the remote's branches so that it could be reviewed and, if everything is good, merged to the remote's `master`. 

In this specific course, we will use the branch-based workflow. 

Let's now publish our work.

### ⌨️ Activity

- Push the changes to the remote branch with the same name as our working branch

<details><summary>Show the commands...</summary>

```bash
git push --set-upstream origin feature
```
</details>


<!-- responses\01_branch_created.md -->

## 👀 Create a pull request. 

Create a pull request named **Steps review**. Set `feature` as the head branch and `master` branch of your forked repository as the base branch.
> 👉 Make sure that you set `master` in your **forked repository** as base, I will not answer to pull requests to the course starter repository.

In GitHub slang, "base" branch is the branch you base your work on, and "head" branch is the branch with the work you are merging.

<!-- responses\02_pull_request.md -->

## 💬 Discuss the changes, add more commits as discussion continues  

### Pull requests

Pull request(PR) is a way to discuss, review and document code. Pull requests are named after a common way of integrating individual contributions into shared codebase. Normally, a person clones a remote official project repository and works on the code locally. After, he pushes the code into his personal remote repository and asks the official repository's maintainers to <strong>pull</strong> his code into their local repositories where they review and potentially <strong>merge</strong> it. The same concept is known under different names, e.g., "merge request".  

Actually, you don't have to use pull request feature of GitHub or similar platforms. Teams can use other modes of communication including face-to-face communication, voice calls, or e-mails, but there is a number of reasons to use such forum thread-like pull requests. Here are some of them:
- to organize discussions related to specific changes in code;
- as a place to show feedback on work in progress, from both auto tests and colleagues;
- to formalize code reviews;
- to make it possible to figure out the reasons and considerations behind a given piece of code later. 

You generally create a pull request when you need to discuss something or receive feedback. For example, if you work on a feature that might be implemented in a number of ways, you might want to create a pull request even before writing the first line of code to share your ideas and to discuss your plans with the collaborators. If the work is more straightforward, the pull request is opened when something is already done, committed, and can be discussed. In some scenarios, you might open a PR just for the sake of quality assurance: to trigger auto tests or to initiate a code review. Whatever you decide, don't forget to @mention persons whose approval is required in your pull request.

Usually, you do the following when creating a PR.
- Specify what you suggest to merge where.
- Write a description explaining the purpose of the pull request. You might want to:
  - add anything important that isn't obvious from the code and anything  helpful to understand the context such as relevant #issues and commit numbers;
  - @mention everyone you want to start collaborating with, or you can @mention them in a comment later;
  - ask your colleagues to do or check something specific.

After you open a PR, whatever automated tests scheduled are run on your code. In our case, this is going to be the same test suite as we ran locally, but in a real project, there might be extra tests and checks.

Please, wait until the tests finish running. You can see the status of the tests at the bottom of the PR discussion thread. Proceed when the tests are done.

<!-- responses\03_remark_issue.md -->

## ⚠️ Add remark about the CI steps being opinionated

The list used in this course is opinionated, we should add a remark stating this.


### ⌨️ Activity: Create a pull request for the remark

1. Check out `master` branch.
2. Create a branch named `bugfix`.
3. Add the remark text at the bottom of `ci.md`.
  ```
  > **GitHub flow** is sometimes used as a nickname to refer to a flavor of trunk-based development  
    when code is deployed straight from feature branches. This list is just an interpretation  
    that I use in my [DevOps courses](http://redpill.solutions).  
    The official tutorial is [here](https://guides.github.com/introduction/flow/).
  ```
4. Commit the changes.
5. Push branch `bugfix` to the remote.
6. Create a pull request named **Adding a remark** with head branch `bugfix` and base branch `master`.

> 👉 Make sure that you set `master` in your **forked repository** as base, I will not answer to pull requests to the course starter repository.

This is what your repository should look like.  
![Continuous Integration: A bugfix](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/3.gif)
  

<details><summary>Show the commands...</summary>

```bash
# Check out master branch. Create a branch named bugfix.
git checkout master

# Create a branch named bugfix-remark.
git checkout -b bugfix

# Add the remark text at the bottom of ci.md.

# Commit the changes
git add ci.md
git commit -m "Add a remark about the list being opinionated"

# Push branch bugfix to the remote.
git push --set-upstream origin bugfix

# Create a pull request using GitHub UI as described above
```
</details>

## 🔀 Merge the "Adding a remark" pull request

### ⌨️ Activity 

1. Open the pull request.
1. Click "Merge pull request"
1. Click "Confirm merge"
1. Click "Delete branch" as we no longer need it

This is the commits diagram after the merge.  
![Continuous Integration: Merging the bugfix to master](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/4.gif)
  

<!-- responses\05_continue_working.md -->

## ▶️ Continue working and adding tests

Collaborating on a pull request often results in additional work being requested. Usually, this is a result of a review or a discussion, but for our course, we are going to simulate this by adding another item to our CI checklist.

Some test coverage is usually in place to support Continuous Integration. The tests coverage requirements differ and are usually found in a document like "contribution guidelines". We are going to be straightforward and will add a test for each line in our checklist.

When working through the activity part, first try to commit tests. If you installed the `pre-commit` hook correctly earlier, the test we've just added will fail and nothing will be committed. Please note that that this is how we know that our tests actually check something. Curiously, if we started with the code before tests, tests passing could mean either that the code works as expected or that the tests don't really check anything. Also, if we didn't write tests first, we could forget writing them completely, as nothing would remind us about it.

### Test Driven Development (TDD)

TDD advocates writing tests before code. Usual TDD work process looks like this.

1. Add a test.  
2. Run all tests and see if the new test fails.  
3. Write the code.  
4. Run tests.  
5. Refactor code.  
6. Repeat.  

Because not passing tests are usually displayed in red, and passing displayed in green, the cycle is also known as "red-green-refactor".


### ⌨️ Activity

First try to commit the tests and let them fail, then add and commit the actual checklist text. You will see that tests are passing ("green").
Then, push the new code to the remote and see how tests are run in the pull request and the pull request's status is updated.

1. Checkout branch `feature`.
2. Add the following tests to `ci.test.js` after the last `it(...);` call.
  ```js
    it('5. Merge/rebase commits from master. Make tests pass on the merge result.', () => {
      expect(/.*merge.*commits.*tests\s+pass.*/ig.test(fileContents)).toBe(true);
    });

    it('6. Deploy from the feature branch to production.', () => {
      expect(/.*Deploy.*to\s+production.*/ig.test(fileContents)).toBe(true);
    });

    it('7. If everything is good in production for some period of time, merge changes to master.', () => {
      expect(/.*merge.*to\s+master.*/ig.test(fileContents)).toBe(true);
    });
  ```
3. Try to commit the tests. If the `pre-commit` hook is in place, the commit will fail.  
4. After, add this text to `ci.md`.
  ```
  5. Merge/rebase commits from master. Make tests pass on the merge result.  
  6. Deploy from the feature branch with a sneaky bug to production.
  7. If everything is good in production for some period of time, merge changes to master. 
  ```
5. Add and commit the changes locally.
6. Push the changes to `feature` branch.

Now you should have something like this  
![Continuous Integration: Continue working](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/5.gif)
  

<details><summary>Show the commands...</summary>

```bash

# Checkout branch feature
git checkout feature

# Add tests to ci.test.js as described above

# we add ci.test.js to commit it later
git add ci.test.js

# Try to commit the tests. If the pre-commit hook is in place, the commit will fail.
git commit

# After, add text to ci.md as described above

# Add and commit the changes locally
git add ci.md
git commit -m "Add the remaining CI steps"

# Push the changes to feature branch.
git push

```

</details>

<!-- responses\06_merge_conflict.md -->

## 🚫 Merge Conflict

Navigate to **Steps review** pull request.

Even though we didn't do anything wrong, and tests pass for our code, we still cannot merge branch `feature` into master. This is because another branch `bugfix` was merged into `master` as we were working on the pull request.
This creates situation when remote `master` branch has commits newer than the one we based branch `feature` on. Because of this, we cannot just add commits from `feature` to `master` and fast-forward `master` HEAD to the tip of `feature`. In this situation we need to either merge or rebase `feature` on master. GitHub can actually perform automatic merge if there is no conflicts. Alas, in our situation both branches have concurrent changes to file `ci.md`, a situation known as merge conflict, and we need to resolve it manually. To make history compatible with the remote `master` branch, we can both merge `feature` into `master` or rebase it on `master`.

### To Merge or to Rebase

#### Merge

- Creates an extra merge commit and preserves work history
  - Preserves initial head branch commits with original timestamps and authors
  - Preserves commit SHAs and references to them in pull requests discussion  
- Requires resolving conflicts just once 
- Makes history non-linear
  - History might be hard to read because of lots of branches (think IDE cable)
  - Makes automatic debugging harder, e.g., makes `git bisect` less useful - it's only going to find a merge commit

#### Rebase

- Replays commits from head branch on top of base branch one-by-one
  - New commits with new SHAs are formed resulting in the commits being linked to original pull requests but not to the relevant comments
  - Commits might be recombined and amended in the process or squashed into fewer, even into just one commit
- Might require resolving a number of conflicts 
- Supports linear history
  - Might be easier to read if not too long without a good reason
  - Auto debugging and troubleshooting is somewhat easier: supports `git bisect`, might make automatic rollbacks more granular and predictable 
- Requires force pushing the rebased head branch when used with pull requests

Usually, teams agree to employ the same strategy anytime they need to merge changes. It can be "pure" merge or rebase or somethings in-between like doing interactive rebase locally for the branches not published to remote, but merges for "public" branches. 

We'll use merge here.

### ⌨️ Activity

1. Make sure latest code is pulled to local `master` branch.
1. Check out branch `feature`.
1. Merge `master` branch into it. A merge conflict related to concurrent changes to `ci.md` will be reported.
1. Resolve the conflict so that both our CI list and the remark about it remain.
1. Push the merge commit into remote `feature`.
1. Check the pull request status in GitHub UI, wait until the merge is permitted.


<details><summary>Show the commands...</summary>

```bash
# Make sure latest code is pulled to local `master` branch
git checkout master
git pull

# Check out branch feature
git checkout feature

# Merge master branch into it
git merge master

# A merge conflict related to concurrent changes to ci.md will be reported
# => Auto-merging ci.md
#    CONFLICT (content): Merge conflict in ci.md
#    Automatic merge failed; fix conflicts and then commit the result.

# Resolve the conflict so that both our CI list and the remark about it remain.
# edit ci.md so that it doesn't contain the merge confilict markers
git add ci.md
git merge --continue
# you can leave the default commit message

# Push the merge commit into remote feature.
git push

# Check the pull request status in GitHub UI, wait until the merge is permitted.
```
</details>

## 👍 Good job!

You finished working on the list and now you need to merge the pull request into `master`.

### ⌨️ Activity: 🔀 Merge the "Steps review" pull request

1. Open the pull request.
1. Click "Merge pull request"
1. Click "Confirm merge"
1. Click "Delete branch" as we no longer need it

This is your repo now  
![Continuous Integration: Merge the feature](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/6.gif)
  


<!-- responses\08_bug_issue.md -->

## 💥 An Error was Found in Production

They say "testing can be used to show the presence of bugs, but never to show their absence." Even though we had some tests and they didn't show us any bugs, a sneaky bug made it into production.

In a scenario like this we need to take care of:
  - whatever deployed into production;
  - the code in `master` branch with the bug, which might be used to start new work.

### Rolling Back vs Fixing Forward

"Rolling back" is deploying a known good previous version to production and reverting the code changes containing bug. "Fixing forward" is adding the fix to `master` and deploying a new version as soon as possible. With continuous delivery and good test suite in place, because APIs and database schemes change as code deployed to production production, rolling back is generally much harder and riskier than fixing forward. 

Because reverting does not bring any risk at all for us, we are going the "revert" way to simulate situation when we
- fix the production ASAP;
- make the code in `master` immediately suitable for starting new work.

### ⌨️ Activity

1. Checkout `master` branch locally.
1. Pull the latest changes form the remote.
1. Revert the merge commit of **Steps review** PR in `master`.
1. Push to remote.

This is the history with the merge reverted  
![Continuous Integration: Reverting the merge](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/7.gif)
  


<details><summary>Show the commands...</summary>

```bash
# Checkout master branch locally.
git checkout master

# Pull the latest changes form the remote.
git pull

# Revert the merge commit of Steps review PR in master.
# We are reverting merge commit, so we need to indicate which line we choose to remain
git show HEAD

# let's suppose, the previous master branch tip commit listed first by the previous command
git revert HEAD -m 1
# you can leave the standard commit message

# Push to remote.
git push
```
</details>

### ☝️ Self-check

Make sure `ci.md` no longer contains the text "sneaky bug" after you revert the merge commit.

<!-- responses\09_fix_bug.md -->

## Fix CI list and return it to master

We reverted `feature` merge entirely. Good news is that now we don't have the bug in `master`. Bad news is that our precious continuous integration list is gone too. So, ideally we need to apply fix over commits from `feature` and return them to `master` along with the fix.

We can approach the task differently:
- revert the commit which reverts the merge of `feature` to `master`;
- cherry-pick commits from the former `feature`.

Different teams use different approaches, and we will cherry-pick the commits into a separate branch and will create a separate pull request for the new branch.

### ⌨️ Activity

1. Create a branch called `feature-fix` and checkout it.  
2. Cherry-pick all the commits of the former `feature` into the new branch. Resolve the merge conflicts.  
    
  ![Continuous Integration: Cherry pick feature commits back](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/8.gif)  
3. Add the regression test to `ci.test.js`:  

  ```js
  it('does not contain the sneaky bug', () => {
    expect( /.*sneaky\s+bug.*/gi.test(fileContents)).toBe(false);
  });
  ```  
4. Run tests locally to make sure they don't pass.  
5. Delete text " with a sneaky bug" in `ci.md`.  
6. Add the changes to tests and to the steps list and commit them.  
7. Push the branch to remote.  

You should have something similar as the result  
![Continuous Integration: Fixed feature](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/9.gif)
  


<details><summary>Show the commands...</summary>

```bash
# Create a branch called feature-fix and checkout it.
git checkout -b feature-fix

# Cherry-pick all the commits of the former feature into the new branch.
# use log to figure out hashes for the commits
# - preceding one adding the first list items: C0
# - adding the last items to list: C2
git log --oneline --graph
git cherry-pick C0..C2
# resolve the merge conflicts like you did merging feature-steps into master
# - edit ci.md and/or ci.test.js
# - add the files to index
# - run "git cherry-pick --continue", you can leave the commit message

# Add the regression test to ci.test.js
# Run tests locally to make sure they don't pass.

# Delete text " with a sneaky bug" in ci.md.

# Add the changes to tests and to the steps list and commit them.
git add ci.md ci.test.js
git commit -m "Fix the bug in steps list"

# Push the branch to remote.
git push --set-upstream origin feature-fix

```
</details>

## Create a pull request. 

Create a pull request named **Fixing the feature**. Set `feature-fix` as the head branch and `master` as the base branch.
Please, wait until the tests finish running. You can see the status of the tests at the bottom of the PR discussion thread. 

> 👉 Make sure that you set `master` in your **forked repository** as base, I will not answer to pull requests to the course starter repository.

## 🔀 Merge the "Fixing the feature" pull request

Thanks for the great fix! Please go ahead and merge the request to `master`.

### ⌨️ Activity 

1. Click "Merge pull request"
2. Click "Confirm merge"
3. Click "Delete branch" as we no longer need it

This is what you should have at the point  
![Continuous Integration: Fix merged into master](https://raw.githubusercontent.com/ntaranov/continuous-integration-team-scenarios-text/master/img/10.gif)
  

<!-- responses\12_congrats.md -->

# 🎉 Congratulations!

You performed all the actions which people commonly do during Continuous Integration.  

If you noticed any problem with the course or know how to improve it, please do create an issue in the [course starter repository](https://github.com/ntaranov/continuous-integration-team-scenarios-students). This course also have [an interactive version](https://lab.github.com/ntaranov/common-continuous-integration-team-scenarios) using GitHub Learning Lab as a platform.


> Please share the course with your friends if you found it useful.
  
> The course is licensed under [CC-BY-4.0](https://github.com/ntaranov/continuous-integration-team-scenarios-students/blob/master/LICENSE) (c) Nick Taranov