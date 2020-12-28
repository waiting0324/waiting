pipeline {
    agent {
      label 'agent-1'
    }
    stages {
        stage('檢測環境') {
            steps{
                echo '檢測環境'
                sh label: '', script: '''java -version
                mvn -v
                git version
                docker -v'''
            }
        }
        stage('拉取代碼') {
            steps{
                echo '拉取代碼'
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'c9b7c84c-2013-470b-9f28-0c04bf1c9496', url: 'https://github.com/waiting0324/waiting.git']]])
            }
        }
        stage('編譯構建') {
            steps{
                echo '編譯構建'
                sh label: '', script: 'mvn clean package -Dmaven.test.skip=true jib:build'
            }
        }
        stage('刪除容器') {
            steps{
                echo '刪除容器'
            }
        }
        stage('刪除鏡像') {
            steps{
                echo '刪除鏡像'
            }
        }
        stage('登入 Docker Hub') {
            steps{
                echo '登入 Docker Hub'
            }
        }
        stage('拉取鏡像') {
            steps{
                echo '拉取鏡像'
            }
        }
        stage('運行容器') {
            steps{
                echo '運行容器'
            }
        }
    }
}
