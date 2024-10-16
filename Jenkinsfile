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
        stage('create docker image') {
            steps{
    sh 'docker build -t pranalisawant/healthcare:1.0'
}
        }
        stage{
            steps{
        withCredentials([usernamePassword(credentialsId: 'docker-cre', passwordVariable: 'dockerpass', usernameVariable: 'dockerlogin')]) 
                 sh 'docker login -u ${dockerlogin} -p ${dockerpass}'
        }
            }
        stage('docker push') {
            steps{
               sh 'docker push pranalisawant/healthcare:1.0' 
          }
    }
    stage('AWS login') {
    steps{
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'awsaccess', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')])
    }
    }
}
