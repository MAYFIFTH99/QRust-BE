pipeline {
    agent any

    environment {
        // 사용하시는 Docker 컨테이너 레지스트리 주소를 입력하세요. (예: 'your-docker-hub-id/qrust-be')
        DOCKER_REGISTRY = 'your-docker-registry/qrust-api'
        DOCKER_IMAGE_TAG = "latest" // 또는 빌드 번호 등을 조합하여 고유하게 생성
    }

    stages {
        stage('Checkout') {
            steps {
                // Git 리포지토리에서 소스코드를 가져옵니다.
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Gradle Wrapper를 사용하여 프로젝트를 빌드하고 테스트를 제외합니다.
                // Windows에서는 gradlew.bat, Linux/macOS에서는 ./gradlew를 사용해야 합니다.
                script {
                    if (isUnix()) {
                        sh './gradlew build --no-daemon -x test'
                    } else {
                        bat 'gradlew.bat build --no-daemon -x test'
                    }
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Docker 이미지를 빌드합니다.
                    def dockerImage = docker.build(DOCKER_REGISTRY + ':' + DOCKER_IMAGE_TAG, '-f qrust-api/Dockerfile .')
                    
                    // Docker Hub 또는 다른 레지스트리에 로그인하여 이미지를 푸시합니다.
                    // Jenkins Credentials에 등록된 ID를 사용해야 합니다.
                    docker.withRegistry('https://index.docker.io/v1/', 'your-docker-hub-credentials-id') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Kubernetes 클러스터에 연결하기 위한 설정입니다.
                // Jenkins에 등록된 Kubeconfig 파일을 사용합니다.
                withKubeConfig([credentialsId: 'your-kubeconfig-credentials-id']) {
                    script {
                        // 1. Deployment의 이미지 주소를 방금 푸시한 이미지로 변경합니다.
                        sh "sed -i 's|image: .*|image: ${DOCKER_REGISTRY}:${DOCKER_IMAGE_TAG}|g' k8s/deployment.yaml"

                        // 2. Secret 값을 Jenkins Credentials에서 가져와 Base64 인코딩 후 적용 (보안 강화)
                        // 이 부분은 실제 구현 시 Jenkinsfile에서 Secret을 다루는 로직을 추가해야 합니다.
                        // 예시: sh "kubectl apply -f k8s/secret.generated.yaml"
                        
                        // 3. Kubernetes 리소스들을 배포합니다.
                        sh 'kubectl apply -f k8s/secret.yaml'
                        sh 'kubectl apply -f k8s/configmap.yaml'
                        sh 'kubectl apply -f k8s/deployment.yaml'
                        sh 'kubectl apply -f k8s/service.yaml'

                        // 4. 배포 상태를 확인합니다.
                        sh 'kubectl rollout status deployment/qrust-api-deployment'
                    }
                }
            }
        }
    }

    post {
        always {
            // 파이프라인이 끝나면 항상 실행되는 부분입니다.
            // 임시 파일 정리 등을 할 수 있습니다.
            echo 'Pipeline finished.'
        }
    }
}
