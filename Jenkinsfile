pipeline {
  agent any

  environment {
    TF_IN_AUTOMATION = "true"
    KUBECONFIG_FILE = credentials('kubeconfig-jenkins-sa') // Jenkins Secret File ID
    KUBECONFIG_DIR = '/tmp/kubeconfig'
    KUBECONFIG_PATH = '/tmp/kubeconfig/config'
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
          mkdir -p $KUBECONFIG_DIR
          cp "$KUBECONFIG_FILE" $KUBECONFIG_PATH
          chmod 600 $KUBECONFIG_PATH
          export KUBECONFIG=$KUBECONFIG_PATH
          echo "✅ Kubeconfig prepared at $KUBECONFIG"
        '''
      }
    }

    stage('Terraform Init') {
      steps {
        sh "KUBECONFIG=$KUBECONFIG_PATH terraform init"
      }
    }

    stage('Terraform Validate') {
      steps {
        sh "KUBECONFIG=$KUBECONFIG_PATH terraform validate"
      }
    }

    stage('Terraform Plan') {
      steps {
        sh "KUBECONFIG=$KUBECONFIG_PATH terraform plan -out=tfplan"
      }
    }

    stage('Terraform Apply') {
      steps {
        sh "KUBECONFIG=$KUBECONFIG_PATH terraform apply -auto-approve tfplan"
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
