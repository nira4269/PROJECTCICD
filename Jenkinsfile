pipeline {
    agent any 

    environment {
        SONARQUBE_URL = 'http://100.26.52.20:9000/'
        NEXUS_URL = 'http://100.26.52.20:8081/#browse/browse:maven-releases'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/nira4269/Maven-Web-Project.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh """
                mvn deploy:deploy-file \
                -DgroupId=com.mt \
                -DartifactId=maven-web-project \
                -Dversion=1.2.1-SNAPSHOT \
                -Dpackaging=war \
                -Dfile=target/my-app-1.0.jar \
                -DrepositoryId=nexus \
                -Durl=$NEXUS_URL
                """
            }
        }
    }
}

