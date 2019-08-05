# Part 2 - Setting up a Jenkins Pipeline

## GitHub repository configuration

1. Enable GitHub webhook:
  * Go to your GitHub repository and click on ‘Settings’.
  * Click on Webhooks and then click on ‘Add webhook’.
  * In the ‘Payload URL’ field, paste your Jenkins environment URL. At the end of this URL add /github-webhook/. In the ‘Content type’ select ‘application/json’ and leave the ‘Secret’ field empty.
  * in the ‘Which events would you like to trigger this webhook?‘ choose Pushes.
2. 

# Jenkins del
idi na jenkins: `http://192.168.0.101:8080/` (ali nekaj podobnega)

v new item zbereš pipeline in potem obkukaš GitHub project in tam daš url not (pomagaj si z ? ikonami)
obkjukaš GitHub hook trigger for GITScm polling

spodaj zberi hello world example pipeline


# TODO: konfiguracijo tako, da bo dalal github hook in da bo delal Jenkinsfile iz repota
kako dodamo GitHub webhook
https://dzone.com/articles/adding-a-github-webhook-in-your-jenkins-pipeline
not realy like this - spremenilo se je

# inštaliraj plugin 'GitHub Authentication'

# sledi to in naredi auth token
https://medium.com/@dillson/triggering-a-jenkins-pipeline-on-git-push-321d29a98cf3

TOKEN: daf8ae16-11c9-4ee9-9215-07ea66840da2
       9efce0bf-d302-4d3e-a221-d2d065d439f2


# Set up github project & add webhook
settings -> webhooks -> add webhook
Payload URL: http://<JENKINS_URL_PORT>/github-webhook/
Content type: application/json
Secret: /
Which events would you like to trigger this webhook? Just the push event.
CHECK Active

# v github projekt dodaj datoteko imena Jenkinsfile v root

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
# back to Jenkis

## inštaliraj plugin 'GitHub Authentication'
Manage Jenkins -> Manage Plugins -> Available
poišči __GitHub Authentication__ in inštaliraj

pa green balls si inštaliraj :D (green balls are better than blue)

## settings
Manage Jenkins -> Configure System (to traja 4 ever da se naloži)

**GitHub Servers** sekcija

tu piše kak generiraš credentials in nastaviš
https://medium.com/@dillson/triggering-a-jenkins-pipeline-on-git-push-321d29a98cf3

OBKLJUKAJ MANAGE HOOKS!


## naredi pipeline
Jenkins -> New Item
Zberi pipeline in ga poimenuj
označi github project in prilepi url ala https://github.com/<username>/testProject
(V repotu mora bit jenkinsfile)

pod **Build Triggers** označi __GitHub hook trigger for GITScm polling__

pod **pipeline** zberi __Pipeline script from SCM__
daj Git
isti repo kot zgoraj (razn če bi rad mel Jenkinsfile v drugem repotu)
Credentials -none-
Izberi ketere branche hočeš

nastavi Script Path - če pustiš default mora bit Jenkinsfile v rootu repota

SAVE

potem moraš 1x buildat ročno **Build Now**

testiraj tako, da lokalno narediš spremembo in jo pušaš na github.
mogo bi se triggerat build - če greš na build -> Console Output vidiš tam 




