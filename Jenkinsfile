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

    // İKİ BLOK TEK BİR POST ALTINDA BİRLEŞTİRİLDİ:
    post {
        always {
            echo 'Pipeline işleyişi tamamlandı.'
        }
        success {
            echo '🚀 Analiz başarıyla sonuçlandı ve SonarQube sunucusuna gönderildi!'
            // Slack bildirimi başarı durumunda buradaki yerini aldı
            slackSend(color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME}' [Build #${env.BUILD_NUMBER}] başarıyla tamamlandı! (${env.BUILD_URL})")
        }
        failure {
            echo '❌ Pipeline başarısız oldu, adımları kontrol edin.'
            // Slack bildirimi hata durumunda buradaki yerini aldı
            slackSend(color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME}' [Build #${env.BUILD_NUMBER}] hata verdi! Lütfen kontrol et: (${env.BUILD_URL})")
        }
    }
}