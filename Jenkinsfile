node {
    def app
    stage('Clone repository') {
        // jenkins에 설정한 repo를 clone (jenkins에 미리 credentials 설정을 통해 가능)
        checkout scm
    }
    stage('Build image') {
       // docker.build 사용을 위해선 Docker pipeline 플러그인 필요
       app = docker.build("dhkoo93/test")
    }
    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }
    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
        echo "triggering updatemanifestjob"
        build job: 'updateManifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
}
