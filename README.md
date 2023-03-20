# Trunk development hackathon

Goal of hackathon - Find a best deployment model for your application


Currently status of deployment in Pay by Plate We are using branches:
master. feature. hotfix.


1. We do not have feature flags except panic button
2. We do not have automated tests or rather we do not have environment where this test
can be executed
3. We have a complex dark release environment based on tricks on APIM, Istio and AKS.
4. Deployment to QA, Prod-Validation and Prod is manual
5. Deployment require demo and on-site testing

## A little bit of theory
We'd like to take a deeper look on two branching and deployment strategies

### Branching model based on deployment from master branch:
This model requires git branches:
* master 
* feature 
* bugfix

Additional requirements for this mode to enable CD are:
* feature flags
* e2e automated tests
* versioning of microservices to avoid dependecy failures
  
Feature flag will allow to deploy features do production automatically without enabling them and will be enabler for canary deployment.
End to end automated tests are required to automate decision is code can be deployed to production.
Versioning of microservices is keeping system in one state.

### Branching model based on sprint/release branch:
This model requires to create branches in git
* master 
* feature 
* bugfix 
* sprint/release

Additional requirements are
* user demo approval 
* e2e automated testing
In this mode CD will be based on automated deployment to dark release environment and manual approval will be required to deploy to production.

## Preparing environment for hackathon
1. Create a new repository and invite members with write role 2. Copy files from repo folder to newly created repository
Repository should contain files: one code file index.html and few yml files with azure pipelines in pipelines folder.
Readme.md is a good practice file.
Release.md will be used in sprint/release based scenario
```.
├── index.html ├── pipelines
│ ├── azure-pipelines-master-pr.yml
│ ├── azure-pipelines-master.yml
│ ├── azure-pipelines-sprint-pr.yml
│ └── azure-pipelines-sprint.yml ├── readme.md
└── release.md
```
2. Create new project in Azure Devops

2. Create new github service connections named trunk-github 3. Create environments in pipelines section: dev, qa and prod
Deployment in different branching models pipelines setup instruction
Master Branch model
1. Create PR pipeline in Azure DevOps
2. Choose Github yaml
3. Choose existing pipeline and find file azure-pipelines-master-pr.yml
4. Create deployment pipeline in Azure DevOps
5. Save pipeline
6. Rename pipeline to repo name + '-master-pr'
7. Choose Github yaml
8. Choose existing pipeline and find file azure-pipelines-master.yml
9. Save pipeline
10. Rename pipeline to repo name + '-master'
11. Run manually both pipelines
12. Enable branch protection on main/master branch - make a cheks mandatory with repo-
name-master-pr pipeline.
Sprint Branch model
1. Create PR pipeline in Azure DevOps
2. Choose Github yaml
3. Choose existing pipeline and find file azure-pipelines-sprint-pr.yml
4. Create deployment pipeline in Azure DevOps
5. Save pipeline
6. Rename pipeline to repo name + '-sprint-pr'
7. Choose Github yaml
8. Choose existing pipeline and find file azure-pipelines-sprint.yml
9. Save pipeline
10. Rename pipeline to repo name + '-sprint'
11. Run manually both pipelines
12. Enable branch protection on main/master branch - make a checks mandatory with repo-
name-sprint-pr pipeline.
13. Enable approvals on qa and prod environments in Azure DevOps

# Hackathon Scenario 

## Exercise 1:
  
### Timeline
1. Planning: US1 User Story - please add text INDEX in our web page.
2. Task: US1.1 Add text INDEX to index.html.
3. Start Sprint 1.0 -> Goal of sprint: modify index.html by adding text INDEX in body section.
4. Create new branch US1.1/AddtextINDEXtoindex.html
5. Modify index.html file with making a typo, instead of INDEX use IDEX.
     <body> IDEX </body>
6. Commit changes to branch.
7. Pull->Push.
8. Create pull request to master branch
9. Review code and verify checks from automated PR pipeline.
10. Execute E2E Test if exists.
11. Merge changes to master.
12. Deploy to Dev, QA and PRD using azure pipeline.
13. Review state of system.
Commands used in this exercise
 git clone repo_name
 git checkout -b US1.1/Add_text_INDEX_to_index.html
 git commit -a -m 'Added INDEX to index.html'
 git pull
 git push --set-upstream origin US1.1/Add_text_INDEX_to_index.html
## Exercise 2

### Timeline

1. Planning: US2 User Story.
    
 Please modify text INDEX to INDEX-EN in our web page. Add new page index-dk.html
Add new page index-se.html
2. Tasks:
US2.1 Update text INDEX to INDEX-EN in index.html. US2.2 Create index-dk.html
US2.3 Create index-se.html
3. Start Sprint 1.1 -> Goal of sprint : modify index.html and add new index-se and index-dk
4. Create new branch US2.1/ModifyINDEXto_INDEX-EN
5. Create new branch US2.2/Create_index-dk
6. Create new branch US2.3/Create_index-se
7. Developer already completed changes in US2.1/ModifyINDEXto_INDEX-EN and merged changes to master branch. Two other tasks are still open.
     <body> INDEX-EN</body>
8. At this moment user is registering bug. Please fix IDEX to INDEX on index.html
9. Bug is registered BUG1 - Fix INDEX in index.html
10. Create new branch BUG1/FixINDEXin_index.html
11. Fix INDEX-EN to INDEX as requested
12. Merge and deploy
13. Merge and deploy US2.3/Create_index-se
14. Due to bug fixing US2.2 will not be delivered in this sprint - we don't have a time to do it.
15. Review state of system.
Commands used in this exercise
``` git pull
 git checkout -b US2.1/Modify_INDEX_to_INDEX-EN
 git pull
 git checkout -b US2.3/Create_index-se
 ```  
 ```git pull
 git checkout -b BUG1/Fix_INDEX_in_index.html
 git commit -a -m 'US2.1/Modify_INDEX_to_INDEX-EN'
 git pull
 git push --set-upstream origin US2.1/Modify_INDEX_to_INDEX-EN
 git add index-se.html
 git commit -a -m 'US2.3/Create_index-se'
 git pull
 git push --set-upstream origin US2.3/Create_index-se
 git commit -a -m 'BUG1/Fix_INDEX_in_index.html'
 git pull
 git push --set-upstream origin BUG1/Fix_INDEX_in_index.html
 ```
## Exercise 3

### Timeline

1. Planning: US3 User Story
Create header 1 style entries in index, index-se, index-dk with language names: English, Swedish, Danish.
2. Tasks:
US3.1 Update H1 with language names - blocked by US2.2 Create index-dk.html From previous sprint US2.2 Create index-dk.html
3. Start Sprint 1.2 -> Goal of sprint: modify file with H1 language text
4. Create branch US3.1/UpdateH1in_index.html
5. Create branch US3.2/UpdateH1in_index-se.html
6. Can't create branch US3.3/UpdateH1in_index.html as it is blocked by US2.2 7. Developer completed changes in US2.2
8. Other tasks are completed
9. Merge all to master and deploy to Dev, QA and PROD
10. File index-dk is not passing E2E test as language name is Denish not Danish, release
    
to PROD is stopped and bugfix needs to be created. 11. Review state of system.
Commands used in this exercise
``` git pull
 git checkout -b US3.1/Update_H1_in_index.html
 git pull
 git checkout -b US3.2/Update_H1_in_index-se.html
 git commit -a -m 'US3.1/Update_H1_in_index.html'
 git pull
 git push --set-upstream origin US3.1/Update_H1_in_index.html
 git commit -a -m 'US3.2/Update_H1_in_index-se.html'
 git pull
 git push --set-upstream origin US3.2/Update_H1_in_index-se.html
 ```
Cleanup repository commands
If there is a need to restart all exercises you can use command below to clean up and reinitialize repository by removing all commits.
 ```cd repo-directory
 rm .git
 git init
 git add .
 git commit -m 'Initial commit' --allow-empty
 git remote add origin <url>
 git push --force --set-upstream origin master
```      