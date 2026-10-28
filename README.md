# gitops_petclinic · 출결 관리 웹앱 GitOps 배포

> AWS Cloud School 수업 실습. `main`에 푸시하면 이미지 빌드부터 Kubernetes 배포까지 자동으로 이어지게 구성했습니다.

## 흐름

```
git push (main)
   ↓
GitHub Actions
   ├─ Nginx / Tomcat 이미지 빌드 → Amazon ECR 푸시 (태그 = 커밋 SHA 7자리)
   └─ deployment-*.yaml 의 이미지 태그를 새 태그로 바꿔 자동 커밋
   ↓
Argo CD 가 매니페스트 저장소를 동기화 → EKS에 배포 (수업 환경)
```

커밋 기록의 `Update image tag to …` 커밋은 GitHub Actions가 실제로 돌면서 남긴 기록입니다.

## 구성

| 파일 | 내용 |
|---|---|
| `.github/workflows/ci.yaml` | 빌드 → ECR 푸시 → 매니페스트 태그 갱신 |
| `Dockerfile.nginx`, `Dockerfile.tomcat` | 프론트(Nginx) / 앱(Tomcat) 이미지 |
| `ROOT/` | 출결 관리 웹앱 빌드 결과물 (JSP와 컴파일된 Servlet `.class`: 로그인, 출석 코드 생성, 출석 목록). 이 저장소는 배포용이라 Java 소스는 포함하지 않았습니다. |
| `deployment-*.yaml`, `service.yaml`, `frontend-nginx-*.yaml` | Deployment, Service, Ingress, HPA |

## 기술

GitHub Actions · Amazon ECR · Amazon EKS · Argo CD · Docker · Nginx · Tomcat
