node {
    def app

    stage('Clone') {
        checkout scm
    }

    stage('Build') {
        app = docker.build("azizmjd/nginx-isoset:${BUILD_NUMBER}")
    }

    stage('Test') {
        sh "docker run --rm azizmjd/nginx-isoset:${BUILD_NUMBER} wget -qO- http://localhost:80"
    }
}