pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Test') {
            steps {
                bat 'mvn test'
            }
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=ca2b7198e0aa9b067f05e919e8a266a495e43f3e -Dsonar.java.binaries=target" 
                }
                
            }
        }
        stage ('Quality Gate'){
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate false
                }
                
            }
        }
    }
}