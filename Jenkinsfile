pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                checkout scm
            }
        }

        stage('Setup & Run') {
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

                fuser -k 5000/tcp || true

                nohup python app.py > output.log 2>&1 &
                '''
            }
        }
    }
}
