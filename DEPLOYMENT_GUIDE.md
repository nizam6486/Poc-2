# Apache on EKS Deployment Guide

## Quick Start

### 1. Deploy Infrastructure
```bash
ansible-playbook site.yml -e "environment=aws-test" -e "aws_region=us-east-2"
```

### 2. Test Deployment
```bash
ansible-playbook test-deployment.yml -e "environment=aws-test" -e "aws_region=us-east-2"
```

### 3. Access Your Application

**Option 1: LoadBalancer (Recommended)**
- Wait 2-3 minutes for AWS to provision the LoadBalancer
- Get URL: `kubectl get svc apache-web-loadbalancer -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'`
- Access: `http://LOADBALANCER_URL`

**Option 2: NodePort**
- Get node IPs: `kubectl get nodes -o wide`
- Access: `http://NODE_EXTERNAL_IP:30080`

**Option 3: Port Forward (Testing)**
```bash
kubectl port-forward service/apache-web-service 8080:80
# Then access: http://localhost:8080
```

## Troubleshooting

### Check Deployment Status
```bash
kubectl get pods
kubectl get svc
kubectl get nodes -o wide
```

### View Logs
```bash
kubectl logs -l app=apache-web
```

### Common Issues

1. **LoadBalancer stuck in pending**: Check security groups allow HTTP traffic
2. **Pods not starting**: Check node resources with `kubectl describe nodes`
3. **Can't access via NodePort**: Ensure security groups allow port 30080

## Cleanup
```bash
ansible-playbook cleanup.yml -e "environment=aws-test" -e "aws_region=us-east-2"
```