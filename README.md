# 프론트엔드 배포 파이프라인

## 개요

![ex_workflow](./app/img/workflow.png)
GitHub Actions에 작성된 워크플로우는 다음과 같이 배포가 진행되도록 합니다.

1. 저장소를 체크아웃합니다.
2. Node.js 18.x 버전을 설정합니다.
3. 프로젝트 의존성을 설치합니다.
4. Next.js 프로젝트를 빌드합니다.
5. AWS 자격 증명을 구성합니다.
6. 빌드된 파일을 S3 버킷에 동기화합니다.
7. CloudFront 캐시를 무효화합니다.
ㅈ
```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```


## 주요 링크

- S3 버킷 웹사이트 엔드포인트: http://yujinawsbucket.s3-website.us-east-2.amazonaws.com
- CloudFrount 배포 도메인 이름: https://dryk6smjb2f9w.cloudfront.net


## 주요 개념

- GitHub Actions과 CI/CD 도구: 
    - 코드 변경사항이 main 브랜치에 push될 때 자동으로 빌드/배포 실행
    - npm build, AWS 인증, S3 업로드 등의 작업을 자동화
    - deployment.yml 파일로 배포 워크플로우 정의
- S3와 스토리지: 
    - 정적 웹사이트 호스팅 기능 사용
    - 빌드된 프론트엔드 파일들(HTML, CSS, JS 등) 저장
    - 버킷 정책으로 public 접근 설정 필요
- CloudFront와 CDN: 
    - S3의 콘텐츠를 전세계 엣지 로케이션에 캐싱
    - HTTPS 제공 및 트래픽 가속화
    - 사용자와 가까운 서버에서 콘텐츠 제공
- 캐시 무효화(Cache Invalidation): 
    - 새 배포 시 이전 캐시 삭제 필요
    - CloudFront 배포 ID와 파일 경로로 무효화
    - GitHub Actions에서 AWS CLI로 자동화 가능
- Repository secret과 환경변수: 
    - AWS 접근키(Access Key/Secret Access Key) 저장
    - GitHub Settings > Secrets에서 설정
    - 워크플로우에서 ${{ secrets.S3_BUCKET_NAME }} 형태로 사용