pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'chmod +x init.sh'
                sh './init.sh'
            }
        }
        stage('Test') {
            steps {
                sh 'docker exec app python manage.py test'
                sh 'sleep 4'
                sh "curl 10.208.0.7:8000"
                sh 'status=$?'
                echo '$status'
            }
        }

        stage('Deploy') {
            steps {
                
                echo 'Deploying....'
            }
        }
    }
    post {
        success {
            sh 'version=$(git log -n 1 --format="%s" HEAD)'
            sh 'echo $(git log -n 1 --format="%s" HEAD)'
            sh 'chmod +x deploy.sh'
            sh './deploy.sh $version'
            sh 'rm -rf *'
        }
        failure {
            echo 'The Pipeline failed :('
            sh 'rm -rf *'
        }
        always {
            sh 'echo build ended, deleting all resources...'
            sh 'chmod +x ./delete.sh'
            sh './delete.sh'
        }
    }
}
