pipeline {
    agent any

    environment {
        // Define environment variables for remote servers
        SSH_KEY = credentials("ssh-key-aws")
        DEV_SERVER = "ec2_user@34.217.66.229"
        JAR_FILE = "target/demo-0.0.1-SNAPSHOT.jar"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Clean and package the application with Maven
                    sh './mvnw clean package'
                }
            }
        }

        stage('Deploy to Dev') {
            steps {
                script {
                    sh """
                        set -e
                        mkdir -p ~/.ssh
                        ssh-keyscan -H ${DEV_SERVER.split('@')[1]} >> ~/.ssh/known_hosts
                        scp -i ${SSH_KEY} ${JAR_FILE} ${DEV_SERVER}:/tmp/
                        ssh -i ${SSH_KEY} ${DEV_SERVER} 'nohup java -jar /tmp/demo-0.0.1-SNAPSHOT.jar > /dev/null 2>&1 &'
                    """
                }
            }
        }
    }
}
