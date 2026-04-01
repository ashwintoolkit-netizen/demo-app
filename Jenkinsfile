pipeline {
    agent any

    environment {
        JFROG_URL = "http://jfrog-server:8081/artifactory/libs-release-local"
        JFROG_USER = "admin"
        JFROG_PASS = "ASHwin123*"
    }

    triggers {
        pollSCM('* * * * *')   // OR use webhook (better)
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/ashwintoolkit-netizen/demo-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Upload') {
            steps {
                sh '''
                curl -u $JFROG_USER:$JFROG_PASS \
                -T target/demo-app-1.0-SNAPSHOT.jar \
                "$JFROG_URL/demo-app.jar"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ashu@ansible-server \
                "cd /home/ashu/ansible-project && ansible-playbook -i inventory deploy.yml"
                '''
            }
        }
    }
}
