pipeline {
    agent any

    stages {
        stage('Clone repo') {
            steps {
                git 'https://github.com/samihakka/Jenkins-demo-app.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'pip install -r app/requirements.txt'
            }
        }

        stage('Run tests') {
            steps {
                sh 'pytest app/test_app.py'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('flask-demo')
                }
            }
        }

        // Optional: Push to Docker Hub
        // stage('Push to Docker Hub') {
        //     steps {
        //         withDockerRegistry([credentialsId: 'docker-hub-creds', url: '']) {
        //             sh 'docker tag flask-demo yourdockerhub/flask-demo'
        //             sh 'docker push yourdockerhub/flask-demo'
        //         }
        //     }
        // }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 5000:5000 flask-demo'
            }
        }
    }
}
