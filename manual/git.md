## Purpose of this proposal
Having a clearly defined Git branch/commit model as well as a clean and sensical Steam releases will help with consistency for developers, the stability of our product, and mitigate the risk of critical bugs making it to the final product affecting the user’s experience as well as our ability to monetize.

## Notes on the language used
Due to limited resources, it is not a good idea to have any hard rules that could restrict development. For this reason, any time strong language such as ‘must’, ‘only’, etc. is seen it can be assumed that it excludes exceptional circumstances. When breaking one of these ‘must’ rules it is to be done with the understanding that there was no other option and ‘what’ and ‘why’ should be communicated well via Slack so that anyone who would need to know can access that information.
    
‘Critical bug in release’ is one or more of:
-   Causing the game to crash
-   Prevent a user from progressing
-   Offensive content
   
‘Complex patch in release’ is one which requires more than one commit.
-   ‘Critical bug in nightly’ is one or more of:
-   Unable to launch the game
-   Offensive content    

## Steam Releases
Steam releases should be driven by necessity and not by what git branches exist.
  
| Name | Description | Updates | Access | Password |
|--|--|--|--|--|
|Default|The main release that all players will use to experience fully implemented and released features. | Critical Patches. Changes that have been tested in a release candidate for a week. | public | - |
| Previous | Same as Default but one major version behind | - | - | - |
| Release-Candidate | Contains new features and fixes that need to be tested for some time before being promoted to Default. This release allows users to preview upcoming stable features. | Minor bugs. Completed features that need testing. All non-critical fixes | public testers | - |
| Nightly | Contains new features that are being worked on and provide a preview for upcoming features and a testing ground for developers. | Contains WIP features and fixes. | Internal testers | brTestBranch |
| Dev-test | A flexible release that contains anything a developer chooses. Should be for internal testing only or other special cases | - | developers | brBattleRoyaleBattleRifle |
  
## Git Branches
### Main Branches
| Name | Purpose | Commit Strategy | Maps to Steam Release |
|--|--|--|--|
| Release | The production-ready branch. | Release-Patch can be merged into this. Staging can be merged into this. This does not get merged back to anywhere else. No direct commits should be made to release unless under exceptional circumstances with agreement of multiple team members.| Default |
| Release-Patch | A mirror of Release for patches to be made and tested. | Direct commits that fix critical issues. For bigger fixes, a branch can be made and merged back in. Fixes from other branches can be cherry-picked, but only to fix a critical bug. Is merged back into Master(which should propagate changes back to other branches). | - |
| Staging | Used for preparing what is in Master to be promoted to Release. | Patches are made directly here and merged back to Master. Only fixes and hardening should happen here as it will soon be promoted to Release.| Release-Candidate | 
|Master | The main branch that should contain all features, bug fixes, and patches. | All branches(except Release) are merged into this branch to keep it up to date. | Nightly |

### Working Branches

Working branches can be branched from Master for making new features, or from Release-Patch for more complex. Working branches should have descriptive names. Working branches should be frequently updated with any changes in Master to minimize time spent with merge conflicts later.


## Merging

### Best Practices
-   Update working branch first: When merging branch A into branch B, branch B should first be merged into branch A and all conflicts resolved and functionality tested before continuing with the A into B merge. This is particularly important when prefabs are merged or overwritten as problems with prefabs are hard to fix after the fact.
-   Test your local branch well: When merging into a branch that others are working from, ensure that there are no compile errors, everything is running fine in the editor, and if there are a lot of, or large, changes then think about running a build.
    

### Merge Requests(MR)

#### In General
MR based deployment enables easy peer-review of the code/design. Instead of pushing directly through to a branch that others may be relying on, everyone can comment on the changes and ask questions before merging any piece of work.

MRs must be made via the GitLab website.

#### For Release
The Release branch is a production-ready branch and the only commits on Release come from MRs. Instead of pushing directly to Release, changes are made in other branches and then MRs are made to Release.

At least one other developer needs to approve an MR on the release branch.

#### Voluntary MRs
Making MRs to Master is not required or forced but is an option if a developer feels as though they would like to have their work checked over by another developer.

## File Locking
*(Nothing to see here)*

### Git Branch Example Diagram (Very WIP)

![](https://lh3.googleusercontent.com/C5mDoX4U0RF8ETp_Iu6byK-ilEtUqGyYODefyDyEsDPwV6LOiWxxbCiPTD74Nx37PHYHS12g9Fq_TUow4b14uigk-wR97wi0h2y94qmpeKNZqCFIUAoXQMzGzdIR2GwyCFrnyknr)