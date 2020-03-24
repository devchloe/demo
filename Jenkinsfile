pipeline {
    agent any
    tools {jdk "jdk9"}

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '78ce21d6-0656-465b-bf20-ad352b8d66ab', url: 'https://github.com/devchloe/demo.git']]])
            }
        }
        stage('Copy Artifact') {
            steps {
                copyArtifacts filter: '**', fingerprintArtifacts: true, projectName: '/demo/frontend-vue-artifact', selector: lastSuccessful(), target: 'src/main/resources/static'
                dir('src/main/resources/static/dist') {
                    sh 'cp -r ./* ../'
                    deleteDir()
                }
            }
        }
        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }
        stage('Deploy') {
            steps {
                sh 'java -jar build/libs/demo-0.0.1-SNAPSHOT.jar'
            }
        }
    }
}