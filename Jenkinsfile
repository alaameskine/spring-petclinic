 pipeline {
    agent any
    tools {          
          /* Pipeline: Checkout SCM -> Dependency Check -> SonarQube Analysis -> Build -> Artifact goes to Dockerhub -> Email Notification */
        jdk 'Java17'
    }


    stages {
        stage('Dependency Check') {
            steps {
                sh './gradlew dependencyCheckAnalyze --info --nowatch-fs'
            }
        } 

        stage('SonarQube Scan') {
            steps { 
                withSonarQubeEnv('SonarQube') {
                    sh './gradlew sonarqube'
                }
            }
        }

            stage('Build') {
                        steps {
                            sh './gradlew clean build'
                        }
                    }
        
    }
}

