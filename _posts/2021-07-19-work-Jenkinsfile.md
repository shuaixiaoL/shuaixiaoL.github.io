---
layout: post
title: Jenkinsfile使用
tags: jenkins Jenkinsfile
categories: work
---

* TOC 
{:toc}

项目进行分支管理，对不同分支进行跟踪迭代部署。

---

# 使用

在项目目录下创建Jenkinsfile文件即可。  
[详细文档](https://www.jenkins.io/zh/doc/book/pipeline/jenkinsfile/)  
可自行查看文档，并不赘述。

# 代码

```
pipeline {
    agent any
    tools{
        maven 'maven-3.6.3'
		 /* .. 定义工具 .. */
    }
    stages {
        stage('Checkout') {
            steps {
                echo '开始拉取代码...'
                cleanWs()
                git branch: '$BRANCH_NAME', url: 'http://:usernamepasswordadmin123456@xxxx:8080/test/test.git'
				/* .. 定义工具 .. */
            }
        }
        stage('Build') {
            parallel{
                 stage('Build backend') {
                    steps {
                        echo '开始构建后台代码...'
                        sh 'cd test-backend/ && mvn clean install -Dmaven.test.skip=true'
                    }
                }
                stage('Build front') {
                    steps {
                        echo '开始构建前台代码...'
                        sh 'cd test-front/ && cnpm install'
                        sh 'cdtest-front/ && cnpm run build:prod'
                    }
                }
            }
        }
        stage('ssh') {
			when {
                branch 'dev'
            }
			/* .. dev分支才进行部署 .. */
            parallel{
                stage('ssh front') {
                    steps {
                        echo '传输前台文件...'
                        sh 'ssh xxxx "cd /opt/deployment/front/ && rm -rf test-front/*"'
                        sh 'scp -r test-front/dist/* xxxx:/opt/deployment/front/test-front/'
                    }
                }  
                stage('ssh backend') {
                    steps {
                        echo '开始传输后台ssh'
                            sh 'ssh xxxx "cd /opt/deployment/backend/test/ && rm -rf test.jar"'
                            sh 'scp -r test-backend/target/test.jar xxxx:/opt/deployment/backend/test/'
                            sh 'ssh xxxx "cd /opt/deployment/backend/test/ && sh service.sh restart"'
                    }
                }  
            }
			/* .. 脚本可使用 >/dev/null 2>&1 & .. */
			/* .. ssh直接连接远程服务器 .. */
        }
    }
     triggers{ pollSCM ( getTriggers() ) }
}

def getTriggers() {
    if(env.BRANCH_NAME.contains('dev')){
        return '0 7-18/2 * * *'
    }
    else{
        return '0 7-18/2 31 2 *'
    }
    /* .. 不同分支不同触发器 .. */
}

```

# 说明

不同分支可满足不同的业务需求。  
在gitlab上可直接编辑jenkinsfile,极其方便。  




