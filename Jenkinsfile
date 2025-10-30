pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'در حال دریافت کد از گیت‌هاب...'
            }
        }

        stage('Build') {
            steps {
                echo 'در حال کامپایل با Maven...'
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'در حال اجرای تست‌ها...'
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'در حال ساخت JAR...'
                sh 'mvn package -DskipTests'
            }
        }
    }

    post {
        success {
            echo 'بیلد با موفقیت انجام شد!'
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        failure {
            echo 'بیلد شکست خورد!'
        }
        always {
            junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
        }
    }
}
