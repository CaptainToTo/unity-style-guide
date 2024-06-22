# Process
These rules describe how developers should contribute to the same code base throughout a project.

Process changes wildly based on the size of the team. Very little, if any at all, is needed for a team of 3 developers, but at 5 some sort of process is needed. And, the kind of process needed for a team of 5 is very different from what's needed for a team of say 20. The business environment the team is working in also greatly affects process. This part of the guide is the most loose for this reason.

**Teams must decide their process at the beginning of the project.**

The following are recommendations for a good process, which can be changed or ignored based on the team's decision.

## Postmortem Periods

Developers can take part in postmortems at regular intervals during development. Focusing on the engineering side of game development, postmortems should be a reflection on how the last set of features were implemented, and how that could affect implementing the next set of features. This postmortem period is encouraged to be used as time to refactor, optimize, and measure the performance of code. This period should avoid lasting more than a week. If any refactors aren't completed by an certain deadline, then work on them should stop in favor of continuing development. Developers should also take this time to interact with artists, designers, and leads to get a better perspective of what features they will be implementing in the future so they can have a better assessment of what needs to be refactored.

## Version Control

Use it. There can be 2 central branches, "main" which contains the most recent version of the complete game, and "bleeding" which acts as a staging area for changes that will be merged into "main". Whenever a team member creates a branch, they should name it as `[TheirName]-[FeatureBeingMade]`, and create it off of "bleeding". Each team member shouldn't have more than 2 branches active at a time to make sure people aren't spread thin. When changes are ready to be merged, the developer should open a pull request to merge into "bleeding", which at least 1 other developer needs to review. The make code reviews easier, pull requests should not have more than 2,000 lines of changes.

After merging into "bleeding", that branch will be used to make sure the new changes do not cause other errors when combined with new changes from other branches. "bleeding" should be merged into "main" at regular intervals, so long as there are no major bugs in "bleeding".