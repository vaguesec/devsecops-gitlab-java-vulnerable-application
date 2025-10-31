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

        stage('SonarQube Analysis') {
            steps {
                echo 'در حال اسکن کد با SonarQube...'
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        sh '''
                            mvn sonar:sonar \
                                -Dsonar.projectKey=my-java-app \
                                -Dsonar.host.url=http://192.168.71.133:9000 \
                                -Dsonar.token=$sonar
                        '''
                    }
                }
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
            echo 'بیلد و اسکن با موفقیت انجام شد!'
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        failure {
            echo 'بیلد یا اسکن شکست خورد!'
        }
        always {
            junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
        }
    }
}
