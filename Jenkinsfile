pipeline {
    agent any
   
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('git-checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/jaiswaladi246/to-do-app.git'
            }
        }

    stage('Sonar Analysis') {
            steps {
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=URL_OF_SONARQUBE -Dsonar.login=TOKEN_OF_SONARQUBE -Dsonar.projectName=to-do-app \
                   -Dsonar.sources=. \
                   -Dsonar.projectKey=to-do-app '''
               }
            }
           
		stage('OWASP Dependency Check') {
            steps {
               dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DP'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
     

         stage('Docker Build') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'docker') {
                    sh "docker build -t  todoapp:latest -f docker/Dockerfile . "
                    sh "docker tag todoapp:latest jinesh1893/todoapp:latest "
                 }
               }
            }
        }

        stage('Docker Push') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'docker') {
                    sh "docker push  jinesh1893/todoapp:latest "
                 }
               }
            }
        }
        stage('trivy') {
            steps {
               sh " trivy jinesh1893/todoapp:latest"
            }
        }
		stage('Deploy to Docker') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'docker') {
                    sh "docker run -d --name to-do-app -p 6000:6000 jinesh1893/todoapp:latest "
                 }
               }
            }
        }

    }
}
