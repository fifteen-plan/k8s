docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/metrics-server:v0.7.2
docker save -o metrics-server.tar  registry.cn-hangzhou.aliyuncs.com/google_containers/metrics-server:v0.7.2
ctr -n k8s.io image import metrics-server.tar

docker build -t nginx:1.19.6 .
kubeadm init --kubernetes-version=v1.20.0 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/12 --apiserver-advertise-address=10.140.203.230 --ignore-preflight-errors=Swap --ignore-preflight-errors=Numcpu --image-repository cr.loognix.cn/kubernetes
kubectl get pod -A -o wide
kubectl create namespace ns-nginx
kebectl get deployment -n ns-nginx
kubectl describe deployment nginx -n ns-nginx
kubectl get svc -n ns-nginx
curl -L 

kubectl get deployment -n ns-nginx
kubectl scale deployment nginx-nginx --replicas=5 -n ns-nginx

kubectl get hpa -n app-ns
# 若持续达到上限，可适当调高 maxReplicas 或优化应用性能
# 预期输出示例：
# NAME          REFERENCE             TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
# web-app-hpa   Deployment/web-app    0%/50%    1         5         2          15s

# 检查当前资源使用率
kubectl get hpa -n app-ns -w


# 查看 HPA 详情
kubectl describe hpa web-app-hpa -n app-ns
# 常见问题：
# - Deployment 未设置 resources.requests.cpu
# - metrics-server 未正常运行
# - 当前负载低于阈值