pipeline {
    agent any
    environment {
        // Define the Docker Hub credentials ID
        DOCKERHUB_CREDENTIALS = '3f33ee93-1577-4e66-892d-c92692cf66d9'
    }

    stages {
        stage ("Compile") {
            steps {
                sh "./gradlew compileJava"
            }
        }
        stage ("Unit Test"){
            steps {
                sh "./gradlew test"
            }
        }

        stage ("Code coverage") {
            steps {
                sh "./gradlew jacocoTestReport"
                publishHTML (target: [
                    reportDir: 'build/reports/jacoco/test/html',
                    reportFiles: 'index.html',
                    reportName: "Jacoco Report"
                ])
                sh "./gradlew jacocoTestCoverageVerification"
            }
        }

        stage ("Package") {
            steps {
                sh "./gradlew build"
            }
        }

        stage ("Docker build") {
            steps {
                sh "docker build -t eeochoa/calculator ."
            }
        }

        stage ("Docker push") {
            steps {
                script {
                    // Push the Docker image to Docker Hub using credentials
                    withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                        // Login to Docker Hub
                        sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"

                        sh "docker push eeochoa/calculator"
                }
            }
        }

        stage ("Deploy") {
            steps {
                sh "docker run -d --rm -p 8765:8080 --name calculator eeochoa/calculator"
            }
        }

        stage ("Sleep") {
            steps {
                sleep 240
            }
        }
    }

    post {
        always {
            sh "docker stop calculator"
        }
    }
}
