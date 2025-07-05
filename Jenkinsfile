pipeline {
  agent any

  environment {
    TF_IN_AUTOMATION = "true"
    // 这里的 'kubeconfig' 是你 Jenkins Credentials 里 Secret File 的 ID
    KUBECONFIG_FILE = credentials('kubeconfig')
  }

  stages {
    stage('Clone GitHub Repo') {
      steps {
        git 'https://github.com/erlangshen2018/k8s-terraform-jenkins.git'
      }
    }

    stage('Prepare Kubeconfig') {
      steps {
        // 把 Secret File 复制到工作目录，并设置环境变量
        sh '''
          mkdir -p $WORKSPACE/.kube
          cp "$KUBECONFIG_FILE" $WORKSPACE/.kube/config
          export KUBECONFIG=$WORKSPACE/.kube/config
          echo "Kubeconfig prepared at $KUBECONFIG"
        '''
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
