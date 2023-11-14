pipeline {
    agent any
    environment {
        DOCKER_HOST="localhost:52222"
        STAGE_INSTANCE="ubuntu@ec2-13-57-37-232.us-west-1.compute.amazonaws.com"
    }
    stages {
        stage('Setup SSH tunnel') {
        steps {
                script {
                sh "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i ~/.ssh/staging -nNT -L ${DOCKER_HOST}:/var/run/docker.sock ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid"
                sleep 5
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                sh "DOCKER_HOST=${DOCKER_HOST} docker run hello-world"
                sh "DOCKER_HOST=${DOCKER_HOST} docker ps -a" 
                }
            }
        }
}
    post {
        always {
            script {
                sh "pkill -F /tmp/tunnel.pid"
            }
        }
    }
}
