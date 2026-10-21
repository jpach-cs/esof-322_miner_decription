# Class Plan: Merging the Stamina System

## Goal

Combine the best parts of the Alpha and Beta stamina implementations (v0.4.0 stamina logic and v0.4.1 stamina bar) into one version on `main`. Along the way we practice PR review without merging, manual cherry-picking, and repository cleanup.

## Ground Rules

- All the code is in one file (`main.c`), so there are only **two implementations** to compare.
- A PR is used here **for review only**. We do **not** merge the team PRs.
- The final version is built by hand (copy and paste of the best fragments), not with `git merge`.
- Green is reserved for health, so the stamina bar stays **blue**.
- Branch names: never create a branch that is also a "folder" for other branches. Nothing may be created under `feature/stamina_system/...`.

## Roles

| Who | Role |
|---|---|
| Alpha team + Alpha lead | Cherry-pick inside the team into `alpha/feature/stamina_bar/integration` |
| Beta team + Beta lead | Cherry-pick inside the team into `beta/feature/stamina_bar/integration` |
| Robert | Reviewer of both PRs, then facilitator of the final session |
| Dr. Pach | Archive, `feature/stamina_system` branch, final PR to `main`, cleanup |
| Final session | Alpha lead + Beta lead + Robert at the projector. One person drives (keyboard), the others read the code and decide. Rotate the driver between functions. |

## Phase 1: Cherry-picking inside each team

Alpha and Beta work **separately**. Each team sits together with its lead, opens every member's implementation, and pastes the best fragments into its integration branch.

- Alpha target: `alpha/feature/stamina_bar/integration`
- Beta target: `beta/feature/stamina_bar/integration`

Use the criteria table from previous classes: correctness, readability, no magic numbers, fits the requirements, and does not break existing behavior.

**Done when:**

- [ ] The code compiles without warnings.
- [ ] Smoke checklist passes (see below).
- [ ] The branch is pushed to GitHub.

## Phase 2: Archive (Dr. Pach)

Do this when both integration branches are ready and **nothing is merged yet**. Work **outside** the repo folder so the archive does not contain itself.

Create the archive:

    cd ~/archives
    git clone --mirror <REPO_URL> montana-tech-miner.git
    7z a montana-tech-miner-before-stamina-merge.7z montana-tech-miner.git

To restore if needed:

    7z x montana-tech-miner-before-stamina-merge.7z
    git clone montana-tech-miner.git restored

Then create the global branch and commit the archive as its **first commit**:

    git checkout main
    git pull origin main
    git checkout -b feature/stamina_system
    mkdir -p archive
    cp ~/archives/montana-tech-miner-before-stamina-merge.7z archive/
    git add archive/
    git commit -m "chore: add repository archive before stamina merge"
    git push -u origin feature/stamina_system

## Phase 3: Pull Requests for review only

Each team lead opens one PR:

- Alpha: `alpha/feature/stamina_bar/integration` into `feature/stamina_system`
- Beta: `beta/feature/stamina_bar/integration` into `feature/stamina_system`

In both PRs:

- The author **must assign Robert as reviewer** (he is notified by email).
- The description contains the smoke checklist with results.
- **Do NOT click Merge.**

## Phase 4: Code review by Robert

Robert reviews both PRs:

- At least 3 comments per PR: 🟡 should, 🔵 nit, and at least one positive.
- Use **Start a review**, then submit with **Approve** or **Request changes**.
- Comments should say which fragments are better and why, since they feed the next phase.

## Phase 5: Final cherry-picking (projector)

Alpha lead, Beta lead, and Robert sit together with two implementations of one file and the review comments in front of them.

1. Check out `feature/stamina_system`.
2. Open both versions side by side (the Alpha integration file and the Beta integration file).
3. Go function by function. For each one, discuss and decide, citing the criteria table and Robert's comments.
4. Paste the chosen fragments into `main.c` on `feature/stamina_system`.
5. Compile and run the smoke checklist.
6. Commit with all authors credited (see below) and push.

Commit command:

    git add main.c
    git commit -m "feat: combine best parts of alpha and beta stamina implementations

    Co-authored-by: Name Surname <email@example.com>
    Co-authored-by: Name Surname <email@example.com>"
    git push origin feature/stamina_system

> **Important:** do not run `git merge` of the integration branches into `feature/stamina_system`. GitHub would mark the PRs as "Merged". Copy and paste by hand.

## Smoke checklist

- [ ] The game compiles and starts.
- [ ] A jump costs `JUMP_STAMINA_COST` (15).
- [ ] Stamina never goes below 0 or above `PLAYER_MAX_STAMINA` (100).
- [ ] Below the jump cost: movement is slower (-25%) and jumps are lower (-50%).
- [ ] The stamina bar is blue and grows and shrinks with the value.
- [ ] No constants hard-coded in the logic (magic numbers).
- [ ] Existing behavior (movement, collisions) is unchanged.

Run the checklist on the final build, and have someone who did not type the code play the game.

## Phase 6: Close the PRs

On both team PRs click **Close pull request** (not Merge) and leave a comment:

    Closed without merging. Reviewed by @robert. Final result combined
    manually in feature/stamina_system (commit <hash>).

## Phase 7: PR to `main`

1. Open a PR: `feature/stamina_system` into `main`, with Robert as reviewer.
2. Robert does a quick last review.
3. Merge with **Squash and merge** (the archive travels to `main` together with the feature).
4. Everyone updates their local repo (commands below).

Update locally:

    git checkout main
    git pull origin main

Optional version tag (Dr. Pach only):

    git tag -a v0.4.1 -m "feat: stamina system and stamina bar"
    git push origin v0.4.1

## Phase 8: Cleanup

Do this only after Dr. Pach has checked contributions on the individual branches and in the closed PRs.

1. Write the list of branches to delete **before** deleting anything.
2. Review the list with the team leads and Robert. Keep only `main`. Delete `feature/stamina_system` too, because after a squash its history differs from `main` and any further work on it would cause false conflicts.
3. Delete the remote branches. The GitHub Branches page is the safest way for many branches.
4. Each student cleans up locally.

List branches:

    git fetch --prune
    git branch -r

Delete a single remote branch:

    git push origin --delete alpha/feature/stamina_bar/integration

Local cleanup for each student:

    git checkout main
    git pull origin main
    git fetch --prune
    git branch
    git branch -D <leftover_branch>

Use `-D`, not `-d`. After a squash, Git does not see the branch as merged, so the "not fully merged" message is expected.

## Definition of Done

- [ ] Alpha and Beta integration branches compile and pass the smoke checklist.
- [ ] Archive `.7z` is committed in `archive/` and reached `main`.
- [ ] Both team PRs were reviewed by Robert and closed without merging.
- [ ] The final version on `main` compiles and passes the smoke checklist.
- [ ] Unneeded branches are deleted and everyone has pulled `main`.

## What comes next

With a clean `main`, the next step is the restructuring: the C++ migration and MVC split. It starts with **documents only**: a Mermaid class diagram plus a table of file, role (Model/View/Controller), and justification, before anybody moves code.
