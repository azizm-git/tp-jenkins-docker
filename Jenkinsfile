node {
    def app

    stage('Cleanup') {
        sh 'docker rm -f nginx-aziz || true'
    }

    stage('Clone') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("localhost:5000/nginx-aziz")
    }

    stage('Push image') {
        docker.withRegistry('http://localhost:5000') {
            app.push("latest")
        }
    }

    stage('Run image') {
        sh 'docker run -d -p 8090:80 --name nginx-aziz localhost:5000/nginx-aziz'
        sh 'sleep 3'
        sh 'curl http://192.168.170.131:8090'
        sh 'docker stop nginx-aziz'
        sh 'docker rm nginx-aziz'
    }
}
