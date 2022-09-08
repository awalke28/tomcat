pipeline {
    agent {
  label 'awalk'
}

environment {
    registryCredentials = 'docker'
    registry = 'bjgomes/tomcat'
}

    stages {
        stage('git'){
            steps {
                git branch: 'main', url: 'https://github.com/bdgomey/tomcat.git'
            }
        }
        stage('MVNBuild') {
            steps {
                sh "mvn clean install"
            }
        }
        stage('DockerBuild') {
            steps {
                script {
                    dockerImage = docker.build(registry)
                }
            }
        }
        stage('DockerPush') {
            steps {
                script {
                    docker.withRegistry('', registryCredentials) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/*.war'
            sh 'docker image rm $(docker image ls -q)'
        }
    }
}
