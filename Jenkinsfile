pipeline {
    agent any

    stages {
        stage('Cloner le dépôt') {
            steps {
                git url: 'https://github.com/FallouTG20/postman-test.git', branch: 'main'
            }
        }

        stage('Installer Newman') {
            steps {
                bat 'npm install -g newman'
            }
        }

        stage('Exécuter les tests Postman') {
            steps {
                bat 'newman run "fake_api testing.postman_collection.json" -e "Fake_API_Racc.postman_environment.json"'
            }
        }
    }
}
