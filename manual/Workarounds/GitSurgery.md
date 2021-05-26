
# Git Surgery

Helpful tips and tricks to perform complicated git problems.

# How to revert a merged branch.

If you have accidentally merged your working branch into master and pushed it to origin then follow these steps. If you are uncomfortable with doing this then please ask someone who knows how to do it for you.

1. Make an announcement to the team that you've fucked up and need to performance this task. They must stash or back up their changes to limit any work lost. No one is to push to origin until the fix is made.

2. Hard reset to the commit you would like to revert back to. Ideally this will be the commit directly before the merge commit.

```
git reset ––hard <hash>
```

3. Create a new temporary branch and checkout

```
git checkout -b <branch-name>
```

4. Cherry-pick any commits into the temp branch that were made on top of the problem merge commit.

```
git cherry-pick <hash>
```

5. Checkout `master` branch again and force push from the hard reset point.

*Note: you may need to unlock this feature on the origin. gitlab doesn't support this by default*

```
git push --force origin master
```

6. Merge in the temp branch and push the new commits.

7. Tell your team they can now update their local history by doing a hard reset to the same one you did in step 2. Then pull the changes. Make sure they **DO NOT PUSH** even though the git client may be asking you to. 

8. Everything should be good now. Happy days!
