pipeline {
    agent any
     tools {
        maven 'MAVEN'  // Must match the name you gave earlier
    }
    options {
            skipStagesAfterUnstable()
        }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
                    steps {
                        sh 'mvn test'
                    }
                    post {
                        always {
                            junit 'target/surefire-reports/*.xml'
                        }
                    }

        }
       stage('Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'agent1', keyFileVariable: 'KEY')]) {
                    sh '''
                        scp -i $KEY target/*.jar ec2-user@ec2-65-2-79-185.ap-south-1.compute.amazonaws.com:/home/ec2-user/
                    '''
                }
            }
        }
    }
}
