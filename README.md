# Jenkins + Terraform 自动部署 Kubernetes 应用

本项目演示如何使用 Jenkins 自动运行 Terraform 脚本，将一个 Nginx 应用部署到 Kubernetes 集群。

## 项目结构

- `main.tf`：Terraform 主模块，声明 provider
- `provider.tf`：Kubernetes provider 配置
- `deployment.tf`：Nginx Deployment 定义
- `service.tf`：Nginx Service 定义
- `Jenkinsfile`：Jenkins 流水线脚本

## 使用说明

1. Jenkins 节点需配置好 kubeconfig（~/.kube/config）
2. Jenkins 安装 Terraform CLI，建议 v1.0+
3. 创建流水线 Job，绑定 GitHub 仓库
4. 手动或 webhook 触发构建

## 效果验证

使用以下命令查看部署结果：

```bash
kubectl get svc,pods
```

## 后续扩展建议

- 使用 Helm Provider 部署复杂组件
- 添加远程状态（S3 + DynamoDB）
- 支持多环境（dev/stage/prod）
- 配合 GitOps 实现全自动 CI/CD











