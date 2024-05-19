pipeline {
    agent any

    environment {
        // Додаємо креденшіали для Docker
        DOCKER_CREDENTIALS_ID = '41b2ffb4-52fd-4180-849d-8509164cdaca'
        CONTAINER_NAME = 'kristine2208/jen'
    }
   

    stages {
        
        
       stage('Вхід у Docker') {
            steps {
                script {
                    // Використовуємо креденшіали з Jenkins для входу в Docker
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Білд Docker зображення') {
            steps {
                script {
                    // Будуємо Docker зображення
                    sh 'docker build -t kristine2208/jen${BUILD_NUMBER} .'
                }
            }
        }

          stage('Тегування Docker зображення') {
            steps {
                script {
                    // Додаємо тег 'latest' до збудованого образу
                    sh 'docker tag kristine2208/jen${BUILD_NUMBER} kristine2208/jen'
                }
            }
        }

        stage('Пуш у Docker Hub') {
            steps {
                script {
                    // Пушимо зображення на Docker Hub
                    sh 'docker push kristine2208/jen${BUILD_NUMBER}'
                    sh 'docker push kristine2208/jen'
                }
            }
        }


        
        stage('Запуск Docker контейнера') {
            steps {
                script {
                    // Запускаємо Docker контейнер з новим зображенням
                    sh 'docker run -d -p 8081:80 --name ${CONTAINER_NAME} --health-cmd="curl --fail http://localhost:80 || exit 1" kristine2208/jen${BUILD_NUMBER}'

                }
            }
        }
    }
}
