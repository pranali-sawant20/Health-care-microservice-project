pipeline{
    agent any
    stages{
        stage('checkout the code from github'){
            steps{
                 git url: 'https://github.com/pranali-sawant20/Health-care-microservice-project.git'
                 echo 'github url checkout'
            }
        }
        stage('codecompile'){
            steps{
                echo 'starting compiling'
                sh 'mvn compile'
            }
        }
        stage('package'){
            steps{
                sh 'mvn package'
            }
        }
    }
}
