pipeline {
    agent any

    environment {
        PATH = "${env.PATH};C:\\WINDOWS\\system32\\config\\systemprofile\\AppData\\Roaming\\npm"
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git 'https://github.com/FallouTG20/postman-test.git'
            }
        }

        stage('Installer Newman') {
            steps {
                bat 'npm install -g newman newman-reporter-html'
            }
        }

        stage('Exécuter les tests Postman') {
            steps {
                // Test 1 : Fake API
                bat 'newman run "fake_api testing.postman_collection.json" -e "Fake_API_Racc.postman_environment.json" -r html --reporter-html-export fake_api_report.html'

                // Test 2 : JSON Placeholder
                bat 'newman run "test API JSONplace_holder.postman_collection.json" -e "JsonPlaceholder.postman_environment.json" -r html --reporter-html-export json_placeholder_report.html'
            }
        }
    }

    post {
        always {
            // Archiver les fichiers HTML
            archiveArtifacts artifacts: '*.html', fingerprint: true

            // Publier rapport Fake API
            publishHTML(target: [
                reportName: 'Rapport Fake API',
                reportDir: '.',
                reportFiles: 'fake_api_report.html',
                keepAll: true,
                alwaysLinkToLastBuild: true,
                allowMissing: false
            ])

            // Publier rapport JSON Placeholder
            publishHTML(target: [
                reportName: 'Rapport JSON Placeholder',
                reportDir: '.',
                reportFiles: 'json_placeholder_report.html',
                keepAll: true,
                alwaysLinkToLastBuild: true,
                allowMissing: false
            ])
        }
    }
}
