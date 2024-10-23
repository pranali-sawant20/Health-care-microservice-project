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
      sh 'docker build -t pranalisawant/healthcare:1.0 .'
         }
        }
        stage('docker login'){
            steps{
        withCredentials([usernamePassword(credentialsId: 'docker-cre', passwordVariable: 'dockerpass', usernameVariable: 'dockerlogin')]){
              sh 'docker login -u ${dockerlogin} -p ${dockerpass}'
        }
            }
            }
        stage('docker push') {
            steps{
               sh 'docker push pranalisawant/healthcare:1.0' 
          }
    }
     stage('aws-login'){
         steps{
   withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'awsaccess', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
            }
         }
     }
    stage('Terraform Operations for test workspace') {
      steps {
        script {
          sh '''
            terraform workspace select test || terraform workspace new test
            terraform init
            terraform plan
            terraform destroy -auto-approve
          '''
        }
      }
    }
   stage('Terraform destroy & apply for test workspace') {
      steps {
        sh 'terraform apply -auto-approve'
      }
    }
   stage('get kubeconfig') {
      steps {
        sh 'aws eks update-kubeconfig --region us-east-1 --name test-cluster'
        sh 'kubectl get nodes'
      }
    }
    stage('Deploying the application') {
      steps {
        sh 'kubectl apply -f app-deploy.yml'
        sh 'kubectl get svc'
      }
    }
   stage('Terraform Operations for Production workspace'){
     when{
      expression{
         return currentBuild.currentResult == 'SUCCESS'
       }
      }
   steps {
    script {
       sh '''
          terraform workspace select prod || terraform workspace new prod
          terraform init
          terraform plan
          tarraform destroy -auto-approve
    '''
    }
   }
  }
    stage('Terraform destroy & apply for production workspace'){
     steps {
       sh 'terraform apply -auto-approve'
    }
      }
    stage('get kubeconfig for production') {
     steps {
       sh 'aws eks update-kubeconfig --region us-east-1 --name prod-cluster'
       sh 'kubectl get nodes'
     }
    }
    stage('Deploying the application to production') {
     steps{
       sh 'kubectl apply -f app-deploy.yml'
       sh 'kubectl get svc'
     }
     }
  }
}

