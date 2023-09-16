
@Library('jenkins-shared-library')
def gv



pipeline {
    agent any
    tools {
    	maven 'mvn'
    }
    stages {
        stage("increment version") {
            steps {
                script {
                    echo 'incrementng version...'
                    sh 'mvn clean package'
                }
            }
        }
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    buildJar()
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
                    buildImage 'rufat51/my-repo:2.0'
                    dockerLogin()
                    dockerPush 'rufat51/my-repo:2.0'
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
                    sshagent(['ec2-server-ky']) {
                    def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"
                        sh "scp docker-compose.yaml ec2-user@54.224.49.214:/home/ec2-user"
                        sh 'ssh -o StrictHostKeyChecking=no ec2-user@54.224.49.214 ${dockerComposeCmd}'
                    }
                }
            }
        }
    }   
}
