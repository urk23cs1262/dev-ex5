pipeline {
    agent any

    stages {

        stage('Create Flask App') {
            steps {
                sh '''
                echo "from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return 'Hello from Jenkins Web App 🚀'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5002)
" > app.py

                echo "flask" > requirements.txt
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                echo "FROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip install flask
EXPOSE 5000
CMD [\\"python\\", \\"app.py\\"]" > Dockerfile

                docker build -t flask-demo .
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f flask-container || true
                docker run -d -p 5000:5002 --name flask-container flask-demo
                '''
            }
        }
    }
}