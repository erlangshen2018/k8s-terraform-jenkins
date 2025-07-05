pipeline {
  agent any

  environment {
    TF_IN_AUTOMATION = "true"
    KUBECONFIG_FILE = credentials('kubeconfig-jenkins-sa') // Jenkins Secret File ID
  }

  stages {
    stage('Clone GitHub Repo') {
      steps {
        git 'https://github.com/erlangshen2018/k8s-terraform-jenkins.git'
      }
    }

    stage('Prepare Kubeconfig') {
      steps {
        sh '''
          mkdir -p $WORKSPACE/kubeconfig
          cp "$KUBECONFIG_FILE" $WORKSPACE/kubeconfig/config
          export KUBECONFIG=$WORKSPACE/kubeconfig/config
          echo "✅ Kubeconfig prepared at $KUBECONFIG"
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
