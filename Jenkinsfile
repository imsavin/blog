pipeline {
    agent any

    stages {
        stage('git') {
            steps {
                git 'https://github.com/imsavin/blog.git'
                sh 'git submodule update --init'
                script {
                  env.GIT_COMMIT_MSG = sh (script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                  currentBuild.displayName = "${GIT_COMMIT_MSG}"
                }
            }
        }
        stage('Build') {
            steps {
                sh '''hugo'''
            }
            
        }
        stage('Propagate') {
        steps {
        sshagent(credentials: ['ssh_blog_publish']) {
      sh '''
      
         rsync -oavz -e "ssh -A root@savin.org.ru ssh"  public/ $WWWUSER@10.8.0.2:/var/www/html/blog/
         ssh -J root@savin.org.ru $WWWUSER@10.8.0.2 chmod -R 755 /var/www/html/blog
      '''
   }
            }
        }
    }
}

