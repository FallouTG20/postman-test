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
                // Exécution de Newman avec rapport HTML
                bat """
                newman run "test API JSONplace_holder.postman_collection.json" \
                -e "Fake_API_Racc.postman_environment.json" \
                -r html \
                --reporter-html-export newman-report.html
                """
            }
        }
    }

    post {
        always {
            // Archive le rapport HTML généré pour consultation depuis Jenkins
            archiveArtifacts artifacts: 'newman-report.html', fingerprint: true

            // Affiche le rapport HTML dans l'interface Jenkins (plugin HTML Publisher requis)
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
