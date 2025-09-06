pipeline {
  agent any

  environment {
    AWS_REGION = "ap-south-1"
    BUCKET_NAME = "your-s3-bucket-name"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/sjbabu/terraform-s3-static-site.git'
      }
    }

    stage('Terraform Init & Apply') {
      dir('terraform') {
        steps {
          sh 'terraform init'
          sh 'terraform apply -auto-approve -var="bucket_name=${BUCKET_NAME}"'
        }
      }
    }

    stage('Upload Static Files to S3') {
      steps {
        sh 'aws s3 cp index.html s3://${BUCKET_NAME}/index.html --acl public-read'
        sh 'aws s3 cp style.css s3://${BUCKET_NAME}/style.css --acl public-read'
      }
    }
  }
}
