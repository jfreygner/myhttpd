pipeline {
    options {
        // set a timeout of 60 minutes for this pipeline
        timeout(time: 60, unit: 'MINUTES')
    }
    agent {
      node {
        label 'master'
      }
    }

    environment {
        DEV_PROJECT = "fre-myhttpd"
        IMAGE_URL = "quay.io/jfreygner/myhttpd:0.1"

        // DO NOT CHANGE THE GLOBAL VARS BELOW THIS LINE
        APP_NAME = "myhttpd"
    }


    stages {

        stage('Launch new app in DEV env') {
            steps {
                echo '### Cleaning existing resources in DEV env ###'
                sh '''
                        oc get project | grep ${DEV_PROJECT} && oc delete project ${DEV_PROJECT}
                        sleep 15
                   '''

                echo '### Creating a new app in DEV env ###'
                sh '''
                        oc new-project ${DEV_PROJECT}
                        oc new-app  --name=${APP_NAME} --image=${IMAGE_URL}
                        oc expose svc ${APP_NAME}
                   '''
            }
        }
    }
}
