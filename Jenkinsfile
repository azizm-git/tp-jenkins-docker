node {
    def REGISTRY_LOCAL  = "localhost:5000/nginx-aziz"
    def DOCKERHUB_IMAGE = "azizmjd/nginx-aziz"
    def GITLAB_IMAGE    = "registry.gitlab.com/azizm-git/aziz_gitlab/nginx-aziz"

    stage('Cleanup') {
        sh 'docker rm -f nginx-aziz-prod || true'
    }

    stage('Clone') {
        checkout scm
    }

    stage('Build') {
        sh "docker build -t ${REGISTRY_LOCAL} ."
    }

    stage('Push Local') {
        sh "docker push ${REGISTRY_LOCAL}"
    }

    stage('Push Docker Hub') {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
            sh "docker login -u $USER -p $PASS"
            sh "docker tag ${REGISTRY_LOCAL} ${DOCKERHUB_IMAGE}"
            sh "docker push ${DOCKERHUB_IMAGE}"
        }
    }

    stage('Push GitLab') {
        withCredentials([usernamePassword(credentialsId: 'gitlab-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
            sh "docker login -u $USER -p $PASS registry.gitlab.com"
            sh "docker tag ${REGISTRY_LOCAL} ${GITLAB_IMAGE}"
            sh "docker push ${GITLAB_IMAGE}"
        }
    }

    stage('Deploy') {
        sh "docker run -d -p 8091:80 --name nginx-aziz-prod ${REGISTRY_LOCAL}"
        sh 'echo "✅ http://192.168.170.131:8091"'
    }
}