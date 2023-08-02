pipeline {
    agent {
        label 'maven'
    }
    stages {
        stage('Clone-code') {
            steps {
                git branch: 'main', url: 'https://github.com/amarsyn/tweet-trend.git'
            }
        }
    }
}