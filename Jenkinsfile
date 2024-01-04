pipeline {
    agent any
    //agent {
      //  docker {
        //    image 'maven:3.9.6-eclipse-temurin-17-alpine'
          //  args '-v /root/.m2:/root/.m2'
        //}
    //}
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
        stage('Deliver') { 
            steps {
                sh "su"
                sh 'whoami'
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}
