pipeline {
  agent any
  environment {
    CODING_DOCKER_REG_HOST = "${env.CCI_CURRENT_TEAM}-docker.pkg.${env.CCI_CURRENT_DOMAIN}"
    CODING_DOCKER_IMAGE_NAME = "${env.PROJECT_NAME.toLowerCase()}/${env.DOCKER_REPO_NAME}/${env.DOCKER_IMAGE_NAME}"
  }
  stages  {
    stage("检出") {
      steps {
        checkout(
          [$class: 'GitSCM',
          branches: [[name: env.GIT_BUILD_REF]],
          userRemoteConfigs: [[
            url: env.GIT_REPO_URL,
              credentialsId: env.CREDENTIALS_ID
            ]]]
        )
      }
    }
        
    stage('安装依赖') {
      steps {
        sh "npm install"
      }
    }

    stage("构建 Docker 镜像") {
      steps {
        // 执行命令构建打包成 Docker 镜像
        sh "hexo g && hexo d"
      }
    }
    
    stage('部署到腾讯云存储') {
      steps {
        sh "coscmd config -a ${env.COS_SECRET_ID} -s ${env.COS_SECRET_KEY}" +
           " -b ${env.COS_BUCKET_NAME} -r ${env.COS_BUCKET_REGION}"
        sh 'rm -rf .git'
        sh 'coscmd upload -r ./ /'
      }
    }
  }
}