# How do we commit and structure our pull requests?

## Our story
The repository is our story and as developers, we are responsible for writing this story well. Anybody who would like to get together with the out story should have a pleasant journey into the code, not the struggle.\
We are the writers of the story and we are the readers in our collaborative work. The way of reading we are talking about is reading code during the code review process or going through the git log for the repository.\
The struggle we have often are the pull requests which are very difficult to read and understand. The goal of this instruction is to address this issue and help to create easy-to-read and easy-to-understand pull requests for review.

## Pull requests
Code review is one of the most important quality gates. Our code review process is based on the assessment of pieces of code worked on in a form of a pull request by other developers than an author.\
If our code and git log is a book, then pull requests may be considered as a chapter. We would like to have our chapter to be about one and only one thing.\
The necessity of having multiple pull requests created for the different repository is obvious and logical, enforced by the tooling we use and common sense.\
The necessity of having multiple pull requests for the UoW (story, bug, PBi, etc) is also an obvious way of adding code, and this line where the majority of developers usually stop. When the unit of work requires a lot of code, difficult itself the pull request tends to be one of those which causes a lot of troubles in reading.\
The solution for the issue is to divide the work into more than one pull request despite it is the work in the same repository for the same UoW. If you can separate your work the way it is safe to merge into a mainline branch directly that's the best option. Otherwise, the following technique may help:
1.	Create your feature branch as a container for your solution
2.	Create a subfeature branch for a piece of work
3.	Do your coding in the subfeature branch and once it's ready to create PR from this sub-feature to the feature branch
4.	Repeat steps 2-3 for all pieces of work you need to do
5.	Once all subfeature branches are merged into feature-branch create PR from the feature branch to your mainline and ask the reviewers to approve PR after spot-check

## Commits
If pull requests are the chapters, the commits are pages of our code-story, they are building blocks and bricks in the git log. We want to have a comprehensive and clear git log and make commits assist us in a code review process.\
In order to achieve this, we need to narrow the volume of commitment to the most reasonable and comprehensive minimum. The extreme states are very dangerous, let us imagine we have 1 commit doing a lot of things which consists of dozens of files. How would we name this commit and what is its purpose? The opposite situation when we will create commit per code line change is also and nightmare, that would garbage our git log and significantly reduce readability.\
The good and proven practice is: to commit as frequently as possible. And this is absolutely fine if this assists developer to be more productive and safe. But this doesn't mean the final look of git history should be as described with dozens of micro commits. Once development is done and code is ready to be assessed by anybody else then author restructuring of commits may be the next step.\
As for the naming, there are different guidances and recommendation exists in the flats of the internet for developers community. We have found this one: https://chris.beams.io/posts/git-commit/ the most comprehensive. In order to save reading time please find the main rules from the article attached.
The seven rules of a great Git commit message
1.	Separate subject from the body with a blank line
2.	Limit the subject line to 50 characters
3.	Capitalize the subject line
4.	Do not end the subject line with a period
5.	Use the imperative mood in the subject line
6.	Wrap the body at 72 characters
7.	Use the body to explain what and why vs. how

Make sure you have given a thought and already split your work into one or more different pull requests.\
If you have split your work into a maximum reasonable amount of PRs and some of those are still hard to swallow in one go then follow the next steps.

## Pull request preparation instruction
Then proceed with the steps:
1.	Once you have finished your development work push your branch to the remote and ensure you have your data persisted in the server. 
2.	Make a soft reset of your branch into the commit you have derived your branch from. E.g. leave all your files intact but reset index into n commits behind. You should see your changes as they were not committed at all.
3.	Create a new branch for pull requests from the following commit. You will cut and put new well-framed commits into this branch and create PR out of it. Let's call the new branch - PR-branch and initial branch with your code - Impl-branch
4.	Divide all work you have done into as much as possible independent and self-explanatory pieces or areas of work inside this one pull request. For instance, the addition of source code of the new library, source code for unit tests for that library, changes in the existing codebase to incorporate classes of that library. Drive yourself with any division logic as long as a piece of the code will be able possible to read and assess independently from the others.
5.	Create first commit for the piece of code. Mind chronological order. E.g. commit with the new codebase library should precede the code with unit tests for it.  Modern git tools will tremendously facilitate this activity by giving you the ability to fast and efficiently separate changes in the same file into several commits. Mind commit naming rules described in the following paragraphs.
6.	Create a pull request with this first commit.
7.	Create the next commit.
8.	Push new commit to remote repository right away. This is needed for AzureDevops to treat each commit as an "update" (it's an internal abstraction of the tool). We bother with it because reviewing PR going through updates is much more convenient than going through commits.
9.	Repeat items 6-7 for all commits you have defined to represent your work.
Once code is reviewed and changes have been requested the author of the code can make the changes in two different ways:
1.	The author can add commits following the rules above by structuring changes in a way they can be reviewed commit-by-commit. That makes sense when new changes don't intersect a lot with ones already presented in PR and are made as additional/missed functionality. 
2.	The author can reshape his commit set by following the steps above again. That can be useful when during PR review process author has been asked(or decided by himself) to refactor at least one of the PR "updates" or even more - to find/pick another way to solve the problem in general.
Once PR is approved, remove your Impl-branch from the remote repository

## Pull request review instruction
Pull request structured the way described above should be reviewed not in way of "all code at once" but on a "per commit" basis. In our case, we use AzureDevops which gives the opportunity to review code commit by commit and leave comments.
1.	Open pull request
2.	Navigate to the first commit
3.	Review the code and leave comments
4.	Navigate into the next commit which contains the next well-framed portion of code
5.	Repeat steps 3-4 if necessary
The changes to the source code will be added with new commits by the author who should follow the same rules of organizing changes in commit with the reviewer in mind.  The reviewer can proceed with the review of the changes in the same manner described.
