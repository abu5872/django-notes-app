pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies in a virtual environment...'
                sh '''
                    python3 -m venv venv
                    ./venv/bin/pip install --upgrade pip
                    ./venv/bin/pip install -r requirements.txt
                    ./venv/bin/pip install --upgrade setuptools
                    ./venv/bin/pip install --upgrade gunicorn
                '''
            }
        }
        stage('Run Application') {
            steps {
                echo 'Starting Django app with Gunicorn...'
                sh '''
                    pkill -f "gunicorn" || true
                    nohup ./venv/bin/gunicorn notesapp.wsgi:application --bind 0.0.0.0:8000 > app.log 2>&1 &
                    sleep 3
                    ps aux | grep gunicorn
                '''
            }
        }
    }
}
