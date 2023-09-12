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
            when {
            	expression {
            		BRANCH_NAME == 'master'
            	}
            }
            steps {
                script {
                    echo "building image"
                    withCredentials([usernamePassword(credentialsId: '0fa9e3bf-37f1-4ddd-977c-dabfafb57f3c', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    	sh 'docker build -t rufat51/my-repo:jma-1.2 .'
                    	sh "echo $PASS | docker login -u $USER --password-stdin"
                    	sh 'docker push rufat51/my-repo:jma-1.2'
                    }
                }
            }
        }
        stage("deploy") {
            when {
            	expression {
            		BRANCH_NAME == 'master'
            	}
            }
            steps {
                script {
                    echo "deploying"
                    //gv.deployApp()
                }
            }
        }
    }   
}
