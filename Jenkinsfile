
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
                    buildImage()
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
                    gv.deployApp()
                }
            }
        }
    }   
}
