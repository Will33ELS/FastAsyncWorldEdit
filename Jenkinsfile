pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/plotsquared-v7']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/IntellectualSites/fastasyncworldedit']])
            }
        }
        stage('Set JDK 17') {
            steps {
                tool name: 'OpenJDK-17.0.1', type: 'jdk'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew clean build'
            }
        }
        stage('Archive artifacts') {
            steps {
                sh 'rm -rf artifacts'
                sh 'mkdir artifacts'
                sh 'cp worldedit-bukkit/build/libs/FastAsyncWorldEdit*.jar artifacts/'
                sh 'cp worldedit-cli/build/libs/FastAsyncWorldEdit*.jar artifacts/'
                archiveArtifacts artifacts: 'artifacts/*.jar', followSymlinks: false
            }
        }
        stage('Fingerprint artifacts') {
            steps {
                fingerprint 'worldedit-bukkit/build/libs/FastAsyncWorldEdit*.jar'
            }
        }
        stage('Publish JUnit test results') {
            steps {
                junit 'worldedit-core/build/test-results/test/*.xml,worldedit-bukkit/build/test-results/test/*.xml'
            }
        }
    }
}