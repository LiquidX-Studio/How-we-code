![image](./img/LiquidXLogo.png)

# LiquidX Developers Handbook
This guide describes the software engineering process at LiquidX.

# How should you use this handbook?
The process described here is largely driven by the strategy described in our [Engineering excellence strategy](./EngineeringExcellenceStrategy.md).
All engineers at LiquidX studio are *required* to follow the instructions included in this handbook.  Any deviation from this engineering process requires written approval from the head of engineering.

# 0. Before you code
Before the coding starts for a new product or feature, do the following:
1. Read the product requirements file and review the UI changes (if any)
2. Read and review the developer design document [design doc template](https://docs.google.com/document/d/1SV8qV3bE6zBeEbqtZ22irpXrOC6RLqWXy58AASRK9VY/edit?usp=sharing)
3. Create appropriate tracking work items on the task board with the work estimates

# 1. Single source of truth
We follow the Trunk-Based development model for version control and have a single source of truth for all software components and modules.  
![img](./img/trunkBasedDevelopment.png)

## Key concepts
- Developers collaborate on the Trunk or the main branch
  - Use feature toggle by [Unleash](https://www.getunleash.io/) to gate your changes so that you never break the build or cause regressions.
- Release branches are created from the main branch
- Release branches are tagged with *major.minor.patch* version number `(000.000.000)`
  - Major version is incremented for new features or breaking changes
  - Minor version number is incremented for incremental feature updates
  - Patch version numbers are incremented for hotfix/patch
- Once the release branch is verified by the QA team, it is deployed to the production
- Release branches are long-lived in case we have to patch/hotfix the production

# 2. Source code repository
Our code is stored in the following repositories:
- [LiquidX studio](https://github.com/LiquidX-Studio)
- [Anime Metaverse](https://github.com/anime-metaverse)
- [Pixelmon](https://github.com/Pixelation-Labs)

> Clone the repo locally to make the change.

# 3. Quality gates
## Static analysis
Before starting the code review use the following static analysis tool to check the formatting and style of your code.

- Typescript/ Javascript - [eslint](https://eslint.org/docs/latest/user-guide/command-line-interface)
- Python [Pyflakes](https://pypi.org/project/pyflakes/)
- Solidity [slither](https://github.com/crytic/slither)

## Unit test coverage
If you are writing backend code (e.g., APIs, web services), your code should have unit test coverage of > 90%.

# 4. Dev testing
You are responsible for testing the code/feature locally and or in the dev environment.

## Pull requests
Once the dev testing is completed, create a pull request to merge your code to the `main` branch.

> Never check in code that will break existing features - use feature flighting!

## Code review
All code checkins require *at least 2* sign-offs from the reviewers before they can be merged into the `main` branch.

# 5. Deployment
## Release branch creation
The devops team will continually create release branches from the main and deploy it to the Release environment.

## Deployment rings 
All code is deployed in the following rings (or environments):
1. Ring 0 - developer's computer
2. Ring 1 - Dev environment
3. Ring 2 - Release environment
4. Ring (Staging) - Staging environment used only for testing Hotfixes
5. Ring 3 - Production environment

Your changes will be deployed in the following path:

> *Ring 0* --> *Ring 1* --> *Ring 2* --> *Ring 3*

# 6. Continuous Integration (CI) and Continuous Delivery (CD)
Our repositories are integrated with the build and deployment pipelines of the [CircleCI](https://circleci.com/) app.

## CI
When a pull request is merged to the main, it triggers the build in our CI/CD tool.  The build will fail if even a single unit test fails.

## CD
- When your pull request is merged to the main, it automatically is deployed in the dev environment
- DevOps team will continually create release branches from the main branch

### Canary release
The risk of deploying new feature or code is minimized by using a [canary release](https://martinfowler.com/bliki/CanaryRelease.html) approach
- Each change is gated by a feature flag and is initially released to a small subset of users
- In case of a regression or major issue, the feature is toggled off and the user experience remains intact
- The feature is made available to everyone once it has been tested in production

# 7. Hotfixes and patching
Only in case of a __critical__ or __major incident__ ([see SLA definition](./EngineeringExcellenceStrategy.md)) we will patch our production deployment.

- Hotfix/patching requires prior approval of the head of engineering 
- Our devops team will create a staging branch from production
- Developers will make changes to that staging branch using a pull request
- The staging branch is deployed in the staging environment for our QA team to test
- Once the dev testing is completed and the QA verification is completed, the change is deployed to the production
- QA team will verify the change in production to ensure there are no regressions
- Finally, the hotfix changes are integrated in the main branch following the regular code review/testing process

# References
- [Trunk-based development](https://trunkbaseddevelopment.com/continuous-integration/)
- [Software development with feature toggles: practices used by practitioners](https://arxiv.org/pdf/1907.06157.pdf) 
