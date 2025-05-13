# TimeDeal Shopping Mall - Kubernetes 설정

이 저장소는 TimeDeal Shopping Mall 애플리케이션의 쿠버네티스 설정 파일들을 포함하고 있습니다.

## 프로젝트 구조

```
.
├── argocd/          # ArgoCD 관련 설정 파일
├── deployment/      # Deployment 설정
├── hpa/            # Horizontal Pod Autoscaler 설정
├── ingress/        # Ingress 설정
├── rbac/           # RBAC(Role-Based Access Control) 설정
├── secret/         # 시크릿 관련 설정
└── service/        # Service 설정
```

## 주요 구성 요소

### 1. Deployment (deployment/timedeal-app-test-deployment.yaml)
- 애플리케이션 컨테이너 배포 설정
- AWS ECR에서 이미지 사용 (820242919524.dkr.ecr.ap-northeast-2.amazonaws.com/shop)
- 리소스 제한 설정:
  - CPU: 요청 100m, 제한 250m
  - 메모리: 요청 512Mi, 제한 1Gi
- AWS Secrets Manager 통합을 위한 CSI 드라이버 설정

### 2. Service (service/timedeal-app-test-service.yaml)
- ClusterIP 타입 서비스
- 포트 80을 컨테이너의 8080 포트로 매핑
- AWS Load Balancer 통합을 위한 어노테이션 포함

### 3. Ingress (ingress/timedeal-app-test-ingress.yaml)
- AWS ALB Ingress Controller 사용
- HTTP(80) 및 HTTPS(443) 포트 지원
- SSL 인증서 통합
- 자동 HTTP to HTTPS 리다이렉션

## 네임스페이스
- 모든 리소스는 `test-app` 네임스페이스에 배포됨

## 보안
- AWS Secrets Manager를 통한 시크릿 관리
- AWS IAM 역할 기반 서비스 계정 사용
- SSL/TLS 암호화 적용

## 배포 환경
- AWS EKS (Elastic Kubernetes Service)
- AWS ECR (Elastic Container Registry)
- AWS ALB (Application Load Balancer)
- AWS ACM (Certificate Manager)

## 접근 방법
- HTTPS를 통한 안전한 접근 (포트 443)
- HTTP 요청은 자동으로 HTTPS로 리다이렉션

## 모니터링 및 스케일링
- HPA(Horizontal Pod Autoscaler) 설정 포함
- 리소스 사용량 모니터링을 위한 리소스 제한 설정

## 주의사항
- 프로덕션 환경 배포 전 시크릿 값 확인 필요
- 리소스 제한 설정 모니터링 필요
- SSL 인증서 만료일 관리 필요 