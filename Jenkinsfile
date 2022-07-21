pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
                git branch: 'main',
                credentialsId: 'bf978538-07cf-49dc-92ae-37ac0d80f0bb',
                url: 'git@gitlab.example.com:gitlab/calculator.git'
            }
        }
        stage("CompileJava") {
            steps {
                sh './gradlew compileJava'
            }
        }
        stage("Test") {
            steps {
                sh './gradlew test'
            }
        }
        stage("Package") {
            steps {
                sh './gradlew build'
            }
        }
        stage("Deploy") {
            steps {
                sh 'cp /var/jenkins_home/workspace/projet-calculator/build/libs/calculator-0.0.1-SNAPSHOT.jar . '
                sh 'docker build -t registry.example.com:5000/projet .'
                sh 'docker push registry.example.com:5000/projet'
            }
        }
    }
    post {
        always {
            emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
        }
    }
}
