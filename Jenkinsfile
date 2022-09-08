pipeline {
    agent {
  label 'awalk'
}

environment {
    registryCredentials = '7233cca8-a617-49ef-820a-657371a8fd81'
    registry = 'auswalk/apachetomcat'
}

    stages {
        stage('git'){
            steps {
                git branch: 'main', url: 'https://github.com/awalke28/tomcat/blob/0034fe74e2b2cb4a2cb8d3e6dfa5dbf8f86b8e5b/Jenkinsfile'
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
