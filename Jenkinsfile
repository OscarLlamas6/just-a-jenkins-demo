pipeline {
    agent { 
        node {
            label 'docker-agent-go'
            }
      }
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('Build') {
            steps {
                echo "Building.."
                sh '''
                go run ./dummy-go-script/main.go
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Testing.."
                sh '''
                go run ./go-hello-world/main.go
                '''
            }
        }
        stage('Deliver') {
            steps {
                echo 'Deliver....'
                sh '''
                echo "doing delivery stuff.."
                '''
            }
        }
    }
}