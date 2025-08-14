pipeline {
    agent any

    tools {
        maven 'Maven-3.9.11'   // Must match Maven name in Jenkins Global Tool Config
        jdk 'JDK-21'           // Must match JDK name in Jenkins Global Tool Config
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    url: 'git@github.com:Bhaskar265/maven-samples.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Generate Reports') {
            steps {
                sh 'mvn site'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.jar', fingerprint: true

            }
        }

        stage('Publish Site Report') {
            steps {
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '/tmp/single-module-site',
                    reportFiles: 'index.html',
                    reportName: 'Project Site'
                ])
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}

