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
                sh 'fuser -k 5000/tcp || true'
            }
        }

        stage('Run App') {
            steps {
                sh '''
                . venv/bin/activate
                nohup /var/lib/jenkins/workspace/vm-app-2_new-branch-1/venv/bin/python app.py > app.log 2>&1 &
                '''
            }
        } // close stage 'Run App'
    } // close stages
} // close pipeline
