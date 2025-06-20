pipeline {
    agent {
        docker {
            image 'python:3.11'  // Python & pip are pre-installed here
        }
    }

    stages {
        stage('Debug Git') {
            steps {
                sh 'git remote -v'
                sh 'git branch -a'
            }
        }
        stage('Clone repo') {
            steps {
                git branch: 'main', url: 'https://github.com/samihakka/Jenkins-demo-app.git'
            }
        }

        stage('Set up venv') {
            steps {
                sh '''
                    python -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r app/requirements.txt
                '''
            }
        }

        stage('Run tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest app/test_app.py
                '''
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
