pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                echo "Updating packages and setting up virtual environment..."
                sudo apt update
                sudo apt install python3-venv lsof -y

                if [ ! -d "venv" ]; then
                    python3 -m venv venv
                fi

                . venv/bin/activate

                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Kill Existing App') {
            steps {
                sh '''
                echo "Killing any existing app running on port 5000..."
                sudo lsof -t -i:5000 | xargs --no-run-if-empty sudo kill -9 || true
                '''
            }
        }

        stage('Run App') {
            steps {
                sh '''
                echo "Starting the app..."
                cd /var/lib/jenkins/workspace/${JOB_NAME}
                . venv/bin/activate
                nohup venv/bin/python app.py > app.log 2>&1 &
                sleep 5

                echo "Checking if app started successfully..."
                if lsof -i:5000; then
                    echo "✅ App started and is listening on port 5000"
                else
                    echo "❌ App failed to start"
                    exit 1
                fi
                '''
            }
        }
    }
}
