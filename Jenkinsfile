pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('git clone') {
            steps {
                git branch: 'feature1', url: 'https://github.com/darshan-bs-2005/java-maven-cicd.git'
            }
        }

        stage('maven build var file') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('docker image push to docker hub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: "docker") {
                        sh '''
                            docker build -t mavenappfeature1:latest.
                            docker tag mavenapp darshanbs2005/mavenappfeature1:latest
                            docker push darshanbs2005/mavenappfeature1:latest
                        '''
                    }
                }
            }
        }

        stage('deploy docker container') {
            steps {
                sh 'docker run -d -p 9002:8080 --name java_container darshanbs2005/mavenappfeature1:latest'
            }
        }

    } // END stages

    post {
        always {
            script {
                def buildStatus = currentBuild.currentResult
                def buildUser = currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause')[0]?.userId ?: 'Github User'

                emailext(
                    subject: "Pipeline ${buildStatus}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: """
                        <p>This is a Jenkins CICD pipeline status.</p>
                        <p>Project: ${env.JOB_NAME}</p>
                        <p>Build Number: ${env.BUILD_NUMBER}</p>
                        <p>Build Status: ${buildStatus}</p>
                        <p>Started by: ${buildUser}</p>
                        <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                    """,
                    to: 'darshanbs189@gmail.com, nnm23ac013@nmamit.in',
                    from: 'darshanbs189@gmail.com',
                    replyTo: 'darshanbs189@gmail.com',
                    mimeType: 'text/html',
                    attachmentsPattern: 'trivyfs.txt'
                )
            }
        }
    }
}

       
      
