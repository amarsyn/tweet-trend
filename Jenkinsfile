pipeline {
    agent {
        label 'maven'
    }
environment {
    PATH = "/opt/apache-maven-3.9.3/bin:$PATH"
}
    stages {
        stage('build') {
            steps {
                echo "---------------- Build Started ----------------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "---------------- Build Completed ----------------"
            }
        }
        stage ('test') {
            steps {
                echo "---------------- UNIT Test Started ----------------"
                sh 'mvn surefire-report:report'
                echo "---------------- UNIT Test Completed ----------------"
            }
        }
        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'valaxy-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('valaxy-sonarqube-server') { // Will pick the global server connection you have configured
                sh '${scannerHome}/bin/sonar-scanner'
            }
            }
        }
        stage("Quality Gate"){
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
                    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                    if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
                }
            }
        }
    }
}