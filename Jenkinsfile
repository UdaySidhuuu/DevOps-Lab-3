pipeline {
    agent any
    tools {
        maven 'MAVEN3'
    }
    stages {
        stage('Build Maven') {
            steps {
                // Explicitly define the SCM step
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/UdaySidhuuu/DevOps-Lab-3']]
                ])
                bat 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    bat 'docker build -t udaysidhu625/devopslab3:latest .'
                }
            }
        }
        stage('Push image to hub') {
            steps {
                script {
                    withCredentials([usernameColonPassword(credentialsId: 'UsernameAndPass', variable: 'USER_PASS')]) {
                        // Splitting the combined user:pass into separate variables
                        String username = USER_PASS.tokenize(':')[0]
                        String password = USER_PASS.tokenize(':')[1]

                        // Login to Docker Hub
                        bat "echo ${password} | docker login -u ${username} --password-stdin"

                        // Push the image to Docker Hub
                        bat 'docker push udaysidhu625/devopslab3:latest'
                    }
                }
            }
        }
    }
}
