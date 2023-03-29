 pipeline {
    agent any
    environment {     
        DOCKERHUB_CREDENTIALS= credentials('dockerhub')     
            } 

    tools {          
          /* Pipeline: Checkout SCM -> Dependency Check -> SonarQube Analysis -> Build -> Artifact goes to Dockerhub -> Email Notification */
        jdk 'Java17'
    }


    stages {
        stage('Dependency Check') {
            steps {
                sh './gradlew dependencyCheckAnalyze --no-watch-fs --info'
            }
        } 

        stage('SonarQube Scan') {
            steps { 
                withSonarQubeEnv('sonarqube') {
                    sh './gradlew sonar'
                }
            }
        }

            stage('Build') {
                        steps {
                            sh './gradlew clean build'
                        }
                    }
    

           /* stage('Deploy to Dockerhub') {
                docker.withRegistry('https://registry.hub.docker.com', dockerhub) {
                    def customImage = docker.build("alaameskine/spring-petclinic")

                     Push the container to the custom Registry 
                    customImage.push()
                } 

            } */

            stage('E-mail notification') {
                    post {
                        success {
                            emailext body: 'The build is SUCCESSFUL', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
                        }
                        
                        failure {  
                            mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "al2a.meskine@gmail.com"; 
                            }
                    }
                }

    }

 }