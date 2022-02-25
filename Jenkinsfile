pipeline {
    agent any
    tools { 
        maven 'Maven 3.6.3' 
        jdk 'jdk11' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
        stage ('Clean') {
            steps {
                echo 'This is a minimal pipeline...'
                sh 'mvn clean'
            }
        }
        stage ('Compile') {
            steps {
                echo 'This is a minimal pipeline...'
                sh 'mvn compile'
            }
        }
        stage ('Test') {
            steps {
                echo 'This is a minimal pipeline...'
                sh 'mvn test'
            }
        }
        stage ('Package') {
            steps {
                echo 'This is a minimal pipeline...'
                sh 'mvn package -DskipTests'
            }
        }
        
        stage ('Undeploy') {
            steps {
                script {
                    echo 'Undeploy previous app...'
                    RESPONSE_KILL = sh(
                        script: 'sudo kill -9 $(ps aux | grep -i target/spring-petclinic | grep -v grep | awk \'{print $2}\')', 
                        returnStatus: true)
                    echo "return status kill: ${RESPONSE_KILL}"
                }    
            }
        }
        stage ('Deploy') {
            steps {
                script {
                    withEnv(['JENKINS_NODE_COOKIE=dontkill']) {
                        sh "java -Dserver.port=8085 -jar target/*.jar --server.servlet.context-path=/petclinic/app &"
                    }
                }
            }
        }
    }
}
