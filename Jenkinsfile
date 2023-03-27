 pipeline {
    agent any
    tools {
        gradle 'Gradle'
        jdk 'Java17'
    }

    stages {
        stage('Build') {
            steps {
                checkout scm
                sh './gradlew clean build'
            }
        }
        stage('Dependency Check') {
            steps {
                sh './gradlew dependencyCheckAnalyze'
            }
        }
        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh './gradlew sonarqube'
                }
            }
        }
    }
}


