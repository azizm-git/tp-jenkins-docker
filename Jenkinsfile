node {
    def app

    stage('Clone') {
        checkout scm
    }

    stage('Build') {
        app = docker.build("azizmjd/nginx-isoset:${BUILD_NUMBER}")
    }

    stage('Test') {
        app.withRun('-p 9090:80') { c ->
            sh 'sleep 2'
            sh 'curl -f http://localhost:9090'
        }
    }
}
