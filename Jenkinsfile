pipeline {

    agent {
        label 'jenkins-agent-1'
    }

    stages {

        stage('Clone Source Code') {
            steps {
                echo 'Cloning source code...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Checking required dependencies...'
                sh 'git --version'
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building application...'
                sh '''
                    test -f index.html
                    mkdir -p build
                    cp index.html build/
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh '''
                    test -f build/index.html
                    grep -q "<html" build/index.html
                '''
            }
        }

        stage('Package Application') {
            steps {
                echo 'Packaging application...'
                sh '''
                    tar -czf cartforge-app.tar.gz -C build index.html
                '''
            }
        }

        stage('Deliver Artifact') {
            steps {
                echo 'Delivering artifact to Jenkins...'
                archiveArtifacts artifacts: 'cartforge-app.tar.gz',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Please check the console log.'
        }
    }
}
