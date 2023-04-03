pipeline {
    /* Pipeline: Checkout SCM -> Dependency Check -> SonarQube Analysis -> JUnit Test -> Build -> Artifact goes to Dockerhub -> Email Notification */
    agent any
    environment {     
        registry = 'alaameskine/spring-petclinic'   
        docker_image = '' 
        DOCKERHUB_CREDENTIALS=credentials('dockerhub_v3')
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
    }
}

            /* This stage has been moved in a brand new Jenkins job as it provides a setup to push & publish the image in Dockerhub
            
            stage('Deploy to Dockerhub') {
                steps {
                    script {            
                             docker.withRegistry('https://registry.docker.hub.com', 'dockerhub_v3') {            
                                dockerImage = docker.build("alaameskine/spring-petclinic")
                           
                                dockerImage.push()
                                            
                                        }
                                 }
                             } */
                      
 
                   /* post {
                        success {
                            emailext body: 'The build is SUCCESSFUL', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
                        }
                        
                        failure {  
                            mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "al2a.meskine@gmail.com"; 
                            }
                    } */