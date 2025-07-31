pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Cloner ton dépôt GitHub (remplace par ton URL)
                git 'https://github.com/ton-utilisateur/ton-depot.git'
            }
        }

        stage('Install Newman') {
            steps {
                // Installer Newman globalement (via npm) si besoin
                bat 'npm install -g newman'
            }
        }

        stage('Run Postman Tests') {
            steps {
                // Lancer la collection avec l'environnement et générer un rapport HTML
                bat '''
                newman run "test API JSONplace_holder.postman_collection.json" ^
                    -e "JsonPlaceholder.postman_environment.json" ^
                    -r cli,html ^
                    --reporter-html-export newman-report.html
                '''
            }
        }

        stage('Archive Reports') {
            steps {
                // Archiver et publier le rapport HTML dans Jenkins
                archiveArtifacts artifacts: 'newman-report.html'
                publishHTML (target: [
                    reportDir: '.',
                    reportFiles: 'newman-report.html',
                    reportName: 'Rapport Newman'
                ])
            }
        }
    }
}
