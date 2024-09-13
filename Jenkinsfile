pipeline {
    agent any 
    tools {
        maven "3.8.5"
    }
    stages {
        stage('Compile and Clean') { 
            steps {
                // Run Maven on a Unix agent.
                sh "mvn clean compile"
            }
        }
        stage('Deploy') { 
            steps {
                sh "mvn package"
            }
        }
        stage('Build Docker image') {
            steps {
                echo "Hello Java Express"
                sh 'ls'
                sh 'docker build -t anvbhaskar/docker_jenkins_springboot:${BUILD_NUMBER} .'
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([string(credentialsId: 'DockerId', variable: 'Dockerpwd')]) {
                    sh "docker login -u anvbhaskar -p ${Dockerpwd}"
                }
            }                
        }
        stage('Docker Push') {
            steps {
                sh 'docker push anvbhaskar/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        stage('Docker Deploy') {
            steps {
                sh 'docker run -itd -p 8081:8080 anvbhaskar/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        stage('Archiving') { 
            steps {
                archiveArtifacts '**/target/*.jar'
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
