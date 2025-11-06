pipeline {
    agent any
    environment {
        // Docker 이미지의 이름과 태그 정의
        DOCKER_IMAGE = 'my-docker-app'
        DOCKER_TAG = "v${env.BUILD_NUMBER}" // Jenkins 빌드 번호를 태그로 사용
    }
    stages {
        stage('1. Checkout Code') { // 명확성을 위해 스테이지 이름을 Checkout으로 변경
            steps {
                echo '=== Git 저장소에서 main 브랜치 코드 Checkout ==='
                
                // ***** 이 부분을 추가합니다. *****
                git url: 'https://github.com/backhyunsug/myproject1.git', // 본인의 Git URL
                    branch: 'main', 
                    credentialsId: '42136afa-0fdb-4db3-9cc7-8db5eadb6982d' // GitHub 접속 자격 증명 ID
                // **********************************
            }
        }
        stage('Build & Test') {
            steps {
                echo '=== 애플리케이션 빌드 및 테스트 ==='
                // 프로젝트 빌드 명령어 실행 (예: mvn clean package)
            }
        }
        stage('Docker Build') {
            steps {
                echo '=== Docker 이미지 빌드 ==='
                script {
                    // Dockerfile을 사용하여 이미지 빌드
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}", '.') 
                }
            }
        }
        stage('Docker Deploy') {
            steps {
                echo '=== Docker 컨테이너 배포 ==='
                script {
                    // 이전 컨테이너 정지 및 삭제 (선택적)
                    sh "docker stop ${DOCKER_IMAGE} || true"
                    sh "docker rm ${DOCKER_IMAGE} || true"
                    
                    // 새 이미지로 컨테이너 실행 (8080 포트와 80 포트를 연결)
                    sh "docker run -d --name ${DOCKER_IMAGE} -p 80:8080 ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }
}
