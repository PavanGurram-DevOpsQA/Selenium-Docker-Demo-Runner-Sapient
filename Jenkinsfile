pipeline {
    agent any
    stages {
        stage('Pull latest image') {
            steps {
                sh "docker pull pavangurram/docker-integration"
            }
        }
        stage('Start Grid') {
            steps {
                sh "docker-compose up --scale chrome=2 --scale firefox=2 --no-color -d hub chrome firefox"
            }
        }
        stage('Run Tests') {
            steps {
                sh "docker-compose up --no-color testng-module-chrome testng-module-firefox"
            }
        }
    }
    post{
        always{
            archiveArtifacts artifacts: 'RunReports/**'
            /* Stop Grid - It is not mentioned as a stage above because when you kill the long running hung
               jobs at stage 'RunTests', it won't execute next following stages and comes directly to 'post' */
            sh "docker-compose down"
        }
    }
}