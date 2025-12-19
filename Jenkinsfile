pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'jdk'
    }

    stages {

        stage('SCM') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/vamshireddy903/Petclinic.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') {
                    sh '''
                      mvn sonar:sonar \
                      -Dsonar.projectKey=sonarqube
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
