pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Postman Tests with Newman') {
            steps {
                bat """
                "C:\\Program Files\\nodejs\\node.exe" "C:\\Users\\fallo\\AppData\\Roaming\\npm\\node_modules\\newman\\bin\\newman.js" run "test API JSONplace_holder.postman_collection.json" -e "Fake_API_Racc.postman_environment.json" -r html --reporter-html-export newman-report.html
                """
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'newman-report.html', fingerprint: true

            publishHTML (target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: 'newman-report.html',
                reportName: 'Rapport Newman Postman'
            ])
        }
    }
}
