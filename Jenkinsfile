pipeline {
    agent any

    environment {
        // Jenkins -> System kısmında tanımladığın SonarQube sunucu adı
        SONAR_SERVER = 'SonarQube' 
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Jenkins -> Tools kısmında tanımladığımız 'SonarScanner' aracı çağrılıyor
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv("${SONAR_SERVER}") {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=techstore"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline işleyişi tamamlandı.'
        }
        success {
            echo '🚀 Analiz başarıyla sonuçlandı ve SonarQube sunucusuna gönderildi!'
        }
        failure {
            echo '❌ Pipeline başarısız oldu, adımları kontrol edin.'
        }
    }
}
post {
    success {
        slackSend(color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME}' [Build #${env.BUILD_NUMBER}] completed başarıyla! (${env.BUILD_URL})")
    }
    failure {
        slackSend(color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME}' [Build #${env.BUILD_NUMBER}] hata verdi! Lütfen kontrol et: (${env.BUILD_URL})")
    }
}
