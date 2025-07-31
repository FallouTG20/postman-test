pipeline {
    agent any

    environment {
        NEWMAN_REPORT_DIR = 'newman'
    }

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
                bat '''
                    if not exist %NEWMAN_REPORT_DIR% mkdir %NEWMAN_REPORT_DIR%

                    newman run "fake_api testing.postman_collection.json" ^
                        -e "Fake_API_Racc.postman_environment.json" ^
                        -r html --reporter-html-export=%NEWMAN_REPORT_DIR%\\fake_api_report.html

                    newman run "test API JSONplace_holder.postman_collection.json" ^
                        -e "JsonPlaceholder.postman_environment.json" ^
                        -r html --reporter-html-export=%NEWMAN_REPORT_DIR%\\json_placeholder_report.html
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'newman\\*.html', allowEmptyArchive: true

            publishHTML([
                reportDir: 'newman',
                reportFiles: 'fake_api_report.html',
                reportName: 'Rapport Fake API',
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true
            ])

            publishHTML([
                reportDir: 'newman',
                reportFiles: 'json_placeholder_report.html',
                reportName: 'Rapport JSON Placeholder',
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true
            ])
        }
    }
}
