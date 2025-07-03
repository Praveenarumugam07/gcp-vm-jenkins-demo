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
                sudo apt install python3-venv -y

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
                echo "Killing existing app on port 5000..."
                sudo kill -9 $(sudo lsof -t -i:5000) || true
                '''
            }
        }

        stage('Run App') {
            steps {
                sh '''
                echo "Starting Flask app for current branch..."
                git branch
                git log -1
                cat app.py

                . venv/bin/activate
                nohup python app.py > app.log 2>&1 &
                '''
            }
        }
    }
}
