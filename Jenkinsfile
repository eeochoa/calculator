pipeline {
    agent any
    stages{
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
                    reportFiles: 'index.html'
                    reportName: "Jacoco Report"
                ])
                sh "./gradlew jacocoTestCoverageVerification"
            }
        }
    }
}
