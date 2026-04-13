node {
    def app
    def REGISTRY_LOCAL = "localhost:5000/nginx-aziz"
    def REGISTRY_DOCKERHUB = "azizmjd/nginx-aziz"
    def REGISTRY_GITLAB = "registry.gitlab.com/azizm-git/aziz_gitlab/nginx-aziz"

    stage('Cleanup') {
        sh 'docker rm -f nginx-aziz-prod || true'
    }

    stage('Clone') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build(REGISTRY_LOCAL)
    }

    stage('Push Registry local') {
        docker.withRegistry('http://localhost:5000') {
            app.push("latest")
        }
    }

    stage('Push Docker Hub') {
        docker.withRegistry('https://registry-1.docker.io', 'dockerhub-creds') {
            app.push("latest")
        }
    }

    stage('Push GitLab Registry') {
        docker.withRegistry('https://registry.gitlab.com', 'gitlab-creds') {
            app.push("latest")
        }
    }

    stage('Deploy') {
        sh 'docker run -d -p 8091:80 --name nginx-aziz-prod localhost:5000/nginx-aziz'
        sh 'echo "✅ Deployed on http://192.168.170.131:8091"'
    }
}