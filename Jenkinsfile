pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git url: 'https://github.com/Pranshu101018/SBDL.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
               sh 'pipenv --python python3 sync'
            }
        }

        stage('Test') {
            steps {
               sh 'pipenv run pytest'
            }
        }

        stage('Package') {
            when {
                anyOf { branch "main"; branch 'release' }
            }
            steps {
               sh 'zip -r sbdl.zip lib'
            }
        }

        stage('Release') {
            when {
                branch 'release'
            }
            steps {
                sh "scp -i /home/pranshu/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf pranshu@40.117.123.105:/home/pranshu/sbdl-qa"
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh "scp -i /home/pranshu/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf pranshu@40.117.123.105:/home/pranshu/sbdl-prod"
            }
        }
    }
}
