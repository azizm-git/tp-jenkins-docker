node {
    def registryProjet = 'registry.gitlab.com/azizm-git/aziz_gitlab/nginx-aziz'
    def IMAGE = "${registryProjet}:version-${env.BUILD_ID}"
    def img

    stage('Clone') {
        checkout scm
    }

    stage('Build') {
        img = docker.build("$IMAGE", '.')
    }

    stage('Run') {
        img.withRun("--name run-${BUILD_ID} -p 9090:80") { c ->
            sh 'curl http://192.168.170.131:9090'
        }
    }

    stage('Push') {
        docker.withRegistry('https://registry.gitlab.com', 'gitlab-creds') {
            img.push('latest')
            img.push()
        }
    }
}