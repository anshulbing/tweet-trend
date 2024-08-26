pipeline {
    agent {
        node{
            label 'maven'
        }
    }
environment {
    PATH = "/opt/apache-maven-3.9.9/bin:PATH"
}
    stages {
        stage('build') {
           steps {
                echo "build started"
                sh "echo 'Testing shell execution'"
                sh "which mvn"
                sh "mvn -version"
                sh "mvn clean deploy"
                echo "build done"
           }
        }
    }
}
