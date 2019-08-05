# Part 2 - Setting up a Jenkins Pipeline with GitHub hook

## GitHub repository configuration

1. Go to your GitHub repository and click on ‘Settings’.
2. Click on Webhooks and then click on ‘Add webhook’.
3. In the ‘Payload URL’ field, paste your Jenkins environment URL. At the end of this URL add /github-webhook/. In the ‘Content type’ select ‘application/json’ and leave the ‘Secret’ field empty.
4. in the ‘Which events would you like to trigger this webhook?‘ choose Pushes.

5. Commit a file called `Jenkinsfile` to the root of your repository. Here is an example:
```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
```
(Check the repo for other Jenkinfiles related to using platformIO and using the GitHub commit status API)

## Jenkins configuration

1. Follow [this](https://medium.com/@dillson/triggering-a-jenkins-pipeline-on-git-push-321d29a98cf3) nice tutorial on how to add your Github credentials to Jenkins.

2. Navigate to http://JENKINS_URL/blue to acces the Blue Ocean interface (easier to use than classic, also prettier)

## Creating a Pipeline

1. Click `New Pipeline` in the upper right corner and follow the instructions. 
2. Jenkins should clone the repository and run the commands as specified in the Jenkinsfile.

## Embedded devices

* To test external devices just plug them into a USB port on the Raspberry Pi running Jenkins. In the Jenkinsfile, specify witch platformio instructions you want to run (building, testing, enviroments, ...).
* To send commit status updates to GitHub, check the Jenkinsfile in this repository.
