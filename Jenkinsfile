def commit_id
pipeline {
    agent any
    stages {
        stage('Preparation') {
            steps {
                script {
                    commit_id = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                }
            }
        }
        stage('Image Build') {
            steps {
                sh "docker build -t dhia829/fleetman-webapp:${commit_id} ."
                sh "docker push dhia829/fleetman-webapp:${commit_id}"
            }
        }
        stage('Deploy') {
            steps {
                sh "sed -i 's|richardchesterwood/k8s-fleetman-webapp-angular:release2|dhia829/fleetman-webapp:${commit_id}|' manifests/deployment.yaml"
                sh 'kubectl apply -f manifests/'
                sh 'kubectl get pods'
            }
        }
    }
}
