pipeline {
    agent any

    environment {
        FLASK_LOG = "output.log"
        VENV_DIR = "venv"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Phanindhraaa/Flask-Hello-World.git'
            }
        }

        stage('Set Up Virtual Environment & Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv $VENV_DIR
                    . $VENV_DIR/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Flask App in Background') {
            steps {
                sh '''
                    . $VENV_DIR/bin/activate
                    pkill -f "python3 app.py" || true
                    nohup python3 app.py > $FLASK_LOG 2>&1 &
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'output.log', fingerprint: true
        }
    }
}

