def registry = 'https://anshulbing.jfrog.io'
def imageName = 'anshulbing.jfrog.io/valaxy-docker-local/ttrend'
def version   = '2.1.4'

pipeline {
    agent {
        node{
            label 'maven'
        }
    }
environment {
    PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
}
    stages {
        stage('build') {
           steps {
                echo "build started with tests"
                sh "mvn -version"
                sh 'mvn clean deploy'
                echo "build done"
           }
        }

        stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"2ee20abb-e709-4c19-b06e-36bcb6f0945a"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "maven-libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    }

            stage(" Docker Build ") {
            steps {
                script {
                echo '<--------------- Docker Build Started --------------->'
                app = docker.build(imageName+":"+version)
                echo '<--------------- Docker Build Ends --------------->'
                }
            }
            }

            stage (" Docker Publish "){
        steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, '2ee20abb-e709-4c19-b06e-36bcb6f0945a'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }

        
    }
}
