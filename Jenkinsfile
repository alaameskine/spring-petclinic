 pipeline {
    /* Pipeline: Checkout SCM -> Dependency Check -> SonarQube Analysis -> JUnit Test -> Build -> Artifact goes to Dockerhub -> Email Notification */
    agent any
    environment {     
        registry = 'alaameskine/spring-petclinic'
        registry_credentials = 'dockerhub'    
        docker_image = '' 
            } 

    tools {          
        jdk 'Java17'
        dockerTool 'Docker'
    }


    stages {
        stage('Dependency Check') {
            steps {
                sh './gradlew dependencyCheckAnalyze --no-watch-fs --info'
            }
        } 

       /* stage('SonarQube Scan') {
            steps { 
                withSonarQubeEnv('sonarqube') {
                    sh './gradlew sonar'
                }
            }
        } */

        stage ('Integration & Unit Testing') {
            steps {
                sh './gradlew test'
                junit '**/test-results/test/*.xml'
            }
        }

            stage('Build') {
                        steps {
                            sh './gradlew clean build'
                        }
                    }
    

            stage('Deploy to Dockerhub') {
                steps {
                    script {
                        dockerImage = docker.build registry + ":$BUILD_NUMBER"
                        docker.withRegistry('https://registry.hub.docker.com', registry_crendentials) {
                        
                            dockerImage.push()
                                 }
                             }
                        }
                    }
                } 

            } 
                   /* post {
                        success {
                            emailext body: 'The build is SUCCESSFUL', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
                        }
                        
                        failure {  
                            mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "al2a.meskine@gmail.com"; 
                            }
                    } */


    
