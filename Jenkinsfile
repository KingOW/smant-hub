pipeline {
    agent any //在任何可用的代理上执行流水线或阶段
    environment {//环境变量
        GIT_PROJECT_NAME = "smant-hub"//项目名称;服务名称
        GIT_CREDENTIALS_ID = "github-user"//git/coding 凭证
        VERSION = sh(script: "echo `date '+%Y%m%d%H'`", returnStdout: true).trim()
        VERSION_ID = "${env.VERSION}-${env.BUILD_ID}"
    }

    tools {
        maven 'jenkins-tool-maven3.9.4'
        jdk 'jenkins-tool-jdk23.0.1'
        nodejs 'jenkins-tool-nodejs14.9.0'
    }

    stages {
        //构建初始化
        stage("Initilization") {
            steps {
                script {
                    echo "描述构建信息;构建id ${env.VERSION_ID}"
                    currentBuild.displayName = "${env.VERSION_ID}"
                    currentBuild.description = "发布${env.GIT_PROJECT_NAME} - ${env.VERSION_ID}"
                }
            }
        }
        //检出代码
//         stage("Check Out") {
//             steps {
//                 echo "========Check Out ${env.GIT_PROJECT_NAME} From Gitee========"
//                 git branch: "${env.GIT_PROJECT_BRANCH}", credentialsId: "${env.GIT_CREDENTIALS_ID}", url: "${env.GIT_PROJECT_ADDR}"
//             }
//         }
        //编译&打包
        stage("Compile&Package") {
            steps {
                echo "========Compile&Package ${env.GIT_PROJECT_NAME} ========"
                // 在有Jenkinsfile同一个目录下（项目的根目录下）
                sh "mvn -B -U -DskipTests clean install deploy"
            }
        }
    }
}