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
                bat 'npm install -g newman newman-reporter-html'
            }
        }

        stage('Exécuter les tests Fake API') {
            steps {
                bat '''
                SET PATH=%APPDATA%\\npm;%PATH%
                newman run "fake_api testing.postman_collection.json" -e "Fake_API_Racc.postman_environment.json" -r html --reporter-html-export fake_api_report.html
                '''
            }
        }

        stage('Exécuter les tests JSON Placeholder') {
            steps {
                bat '''
                SET PATH=%APPDATA%\\npm;%PATH%
                newman run "test API JSONplace_holder.postman_collection.json" -e "JsonPlaceholder.postman_environment.json" -r html --reporter-html-export json_placeholder_report.html
                '''
            }
        }

        stage('Publier les rapports HTML') {
            steps {
                publishHTML([
                    reportName: 'Fake API Report',
                    reportDir: '.',
                    reportFiles: 'fake_api_report.html',
                    keepAll: true,
                    alwaysLinkToLastBuild: true,
                    allowMissing: false
                ])
                publishHTML([
                    reportName: 'JSON Placeholder Report',
                    reportDir: '.',
                    reportFiles: 'json_placeholder_report.html',
                    keepAll: true,
                    alwaysLinkToLastBuild: true,
                    allowMissing: false
                ])
            }
        }
    }
}
