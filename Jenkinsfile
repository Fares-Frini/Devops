pipeline {
    agent any
    
    environment {   
        SONARQUBE_TOKEN = credentials('squ_ca39596688df57a802b1af9e5e60c3998ede834d')  // replace with your credential ID
        SONARQUBE_URL = 'http://192.168.1.21:9000/'  // your SonarQube server URL
    }
    stages {
        stage('Getting code from GITHUB') {
            steps {
                echo 'Pulling ...'
                git branch: 'fares',
                    url: 'https://github.com/Fares-Frini/Devops.git',
                    //credentialsId: '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'
            }
        }

        stage('MVN clean') {
            steps {
                echo 'MVN clean ...'
                sh 'mvn clean'
            }
        }
        
        stage('MVN package') {
            steps {
                echo 'MVN package ...'
                sh 'mvn package -DskipTests'
            }
        }
        
        stage('MVN build') {
            steps {
                echo 'MVN install ...'
                sh 'mvn install -DskipTests'
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests ...'
                sh 'mvn test'
            }
        }

        stage('Jococo & Sonarqube test') {
            steps {
                echo 'Testing and preparing sonar report ...'
                sh """
                    mvn clean test
                    mvn jacoco:report
                    mvn sonar:sonar \
                    -Dsonar.host.url=${SONARQUBE_URL} \
                    -Dsonar.login=${SONARQUBE_TOKEN}
                """
            }
        }

        stage('Nexus Deploy') {
            steps {
                echo 'Deploying to Nexus ...'
                sh 'mvn deploy -X'
            }
        }
    }
}