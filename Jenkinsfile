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
                steps{
                sh label: '', script: '''echo \'檢查容器是否存在\'
                containerid=`docker ps -a | grep -w waiting | awk \'{print
                $1}\'`
                if [ "$containerid" != "" ];then
                echo ‘容器存在，停止容器’
                docker stop $containerid
                echo ‘刪除容器’
                docker rm $containerid
                fi'''
                }
            }
        }
        stage('刪除鏡像') {
            steps{
                echo '刪除鏡像'
                sh label: '', script: '''echo \'檢查鏡像是否存在\'
                imageid=`docker images | grep waiting | awk \'{print $3}\'`
                if [ "$imageid" != "" ];then
                echo \'刪除鏡像\'
                docker rmi -f $imageid
                fi'''
            }
        }
        stage('登入 Docker Hub') {
            steps{
                echo '登入 Docker Hub'
                sh label: '', script: 'docker login -u tony60107 -p s89236001'
            }
        }
        stage('拉取鏡像') {
            steps{
                echo '拉取鏡像'
                sh label: '', script: 'docker pull tony60107/waiting:1.0'
            }
        }
        stage('運行容器') {
            steps{
                echo '運行容器'
                sh label: '', script: 'docker run -itd --name waiting --restart always -p 8080:8080 tony60107/waiting:1.0'
            }
        }
    }
}
