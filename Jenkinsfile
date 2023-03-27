 pipeline {
    agent any
    tools {
        gradle 'Gradle'
        jdk 'Java17'
    }

    stages {
        stage('Build') {
            steps {
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


