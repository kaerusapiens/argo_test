새 설정을 인식시키려면 Argo Workflow 컨트롤러를 재시작해야 합니다:
```
kubectl rollout restart deployment argo-server -n argo
kubectl rollout restart deployment workflow-controller -n argo

kubectl logs deployment/workflow-controller -n argo
```
