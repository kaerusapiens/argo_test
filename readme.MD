새 설정을 인식시키려면 Argo Workflow 컨트롤러를 재시작해야 합니다:
```
kubectl rollout restart deployment argo-server -n argo
kubectl rollout restart deployment workflow-controller -n argo
kubectl logs deployment/workflow-controller -n argo
```

## Argo Workflow 설치 및 실행
Argo Workflow를 설치하고 실행하기 위해 다음 명령어를 사용합니다:
argo namespace를 생성하고 Argo Workflow를 설치합니다:
namespace생성이 필요한 이유는, Argo Workflow가 Kubernetes 클러스터 내에서 리소스를 관리하기 위해 별도의 네임스페이스를 사용하기 때문입니다. 이를 통해 Argo Workflow와 다른 애플리케이션 간의 리소스 충돌을 방지할 수 있습니다.
```bash
kubectl create namespace argo
```
Argo Workflow를 설치하려면 다음 명령어를 사용합니다:
```bash
kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/latest/download/install.yaml
```

port-forward를 사용하여 Argo Workflow UI에 접근할 수 있습니다:
```bash
kubectl port-forward -n argo deployment/argo-server 2746:2746
```
이제 웹 브라우저에서 `http://localhost:2746`에 접속하여 Argo Workflow UI를 확인할 수 있습니다.

## Argo Workflow 예제 실행
다음 명령어를 사용하여 Argo Workflow 예제를 실행할 수 있습니다:
```bash
argo submit --watch https://raw.githubusercontent.com/argoproj/argo-workflows/master/examples
/hello-world.yaml -n argo

argo logs artifact-example-4f6gl -n argo

```
## Argo Workflow 예제 실행 (로컬 파일)
로컬 파일을 사용하여 Argo Workflow 예제를 실행하려면 다음 명령어를
사용합니다:

`argo submit example.yaml -n argo --log`

```bash
argo submit --watch loops-withitems.yaml -n argo
```


```
kubectl apply -f argo-workflow-taskresults-clusterrole.yaml
```


# argoworkflow get podname list
Argo Workflow에서 실행 중인 파드의 이름 목록을 가져오려면 다음 명령어를 사용합니다:
```bash
kubectl get pods -n argo -l workflows.argoproj.io/workflow=example
```

# block style
YAML에서 문자열을 포맷팅하는 방법은 여러 가지가 있습니다. block style은 문자열을 여러 줄로 나누어 작성할 수 있게 해줍니다. YAML에서 큰따옴표로 감싸지 않은 문자열(특히 block style인 |, >, |-, >- 등)은 escape문자를 해석하지 않고 문자 그대로 저장합니다.작은따옴표(')의 원리랑 같
 - `>`은 각 개행을 각 줄이 **공백(스페이스)**로 1줄로 이어붙입니다. 마지막에는 개행이 추가됩니다.
 - `>-`은 각 개행을 공백을 추가하면서 1줄로 이어붙이고 마지막 개행을 제거합니다.
 - `|`은 각 개행을 유지하면서 여러 줄로 이어붙입니다.
 - `|-`은 각 개행을 유지하면서 여러 줄로 이어붙이고 마지막 개행을 제거합니다.
```yaml
value: >-
    Selamat Pagi,
    Selamat Siang
```

```text
Selamat Pagi, Selamat Siang
```


```yaml
value: >
    Selamat Pagi,
    Selamat Siang
```

```text
Selamat Pagi, Selamat Siang

```
### backslash 이스케이프
YAML에서 백슬래시(`\`)를 이스케이프하려면 두 개의 백슬래시(`\\`)를 사용해야 합니다. 예를 들어, 다음과 같이 작성할 수 있습니다:
```yaml
value: |-
    Selamat\"Pagi,
    Selamat\nSiang
    Selamat\\nMalam
    "Selamat\\nMalam"
```

```text
Selamat"Pagi, 
Selamat\nSiang
Selamat\nMalam
```

- folded style
```yaml
value : >
    Selamat\"Pagi,
    Selamat\nSiang
    Selamat\\nMalam
```

```text
Selamat"Pagi, Selamat\nSiang Selamat\nMalam
```



```yaml
value : |-
    Selamat\
    Pagi,
    Selamat\nSiang
    Selamat\\nMalam
```
```test
SelamatPagi,
Selamat\nSiang
Selamat\nMalam
```