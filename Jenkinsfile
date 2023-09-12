def gv

pipeline {
    agent any
    tools {
    	maven 'mvn'
    }
    stages {
        
        stage("build jar") {
            steps {
                script {
                    echo "building jar"
                    sh 'mvn package'
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    echo "building image"
                    withCredentials([usernamePassword(credentialsId: 'fa2753f9-b30c-465d-8730-6cf4a7d5a3f8', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    	sh 'docker build -t rufat51/my-repo:jma-1.2 .'
                    	sh "echo $PASS | docker login -u $USER --password-stdin"
                    	sh 'docker push rufat51/my-repo:jma-1.2'
                    }
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    echo "deploying"
                    //gv.deployApp()
                }
            }
        }
    }   
}
