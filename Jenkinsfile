pipeline {
  agent any

  environment {
    TF_IN_AUTOMATION = "true"
  }

  stages {
    stage('Clone GitHub Repo') {
      steps {
        git 'https://github.com/your-org/k8s-terraform-jenkins.git'
      }
    }

    stage('Terraform Init') {
      steps {
        sh 'terraform init'
      }
    }

    stage('Terraform Validate') {
      steps {
        sh 'terraform validate'
      }
    }

    stage('Terraform Plan') {
      steps {
        sh 'terraform plan -out=tfplan'
      }
    }

    stage('Terraform Apply') {
      steps {
        input message: "Apply Terraform changes?"  // 可交互确认
        sh 'terraform apply -auto-approve tfplan'
      }
    }
  }

  post {
    success {
      echo "✅ 部署成功"
    }
    failure {
      echo "❌ 部署失败，请检查日志"
    }
  }
}
