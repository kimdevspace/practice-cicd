pipeline {
    agent any

    tools {
        jdk 'jdk17'
    }

    stages {
        stage('Checkout') {
            steps {
                // GitHub에서 소스코드 체크아웃
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // gradlew 실행 권한 부여
                sh 'chmod +x ./gradlew'
                // 프로젝트 빌드
                sh './gradlew clean build -x test'
            }
        }

        stage('Test') {
            steps {
                // 테스트 실행
                sh './gradlew test'
            }
        }

        stage('Docker Build') {
            steps {
                // Docker 이미지 빌드
                sh 'docker build -t practice-cicd .'
            }
        }

        stage('Docker Run') {
            steps {
                // 기존 컨테이너 제거
                sh 'docker rm -f practice-cicd || true'
                // 새 컨테이너 실행
                sh 'docker run -d -p 8081:8080 --name practice-cicd practice-cicd'
            }
        }
    }
}
