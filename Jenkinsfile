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
                sh 'mvn jar:jar install:install help:evaluate -Dexpression=project.name'
                sh 'NAME=$(mvn help:evaluate -Dexpression=project.name | grep "^[^\\[]")'
                sh 'VERSION=$(mvn help:evaluate -Dexpression=project.version | grep "^[^\\[]")'
                sh 'java -jar target/my-app-1.0-SNAPSHOT.jar'
            }
        }

        
        stage('Publish to Artifactory') {
            steps {
                script {
                    def server = Artifactory.server 'artifactory'
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "/var/jenkins_home/workspace/demo/target/surefire-reports/*",
                                "target": "example-repo-local/"
                            }
                        ]
                    }"""
                    server.upload spec: uploadSpec
                }
            }
        }
    }
}
