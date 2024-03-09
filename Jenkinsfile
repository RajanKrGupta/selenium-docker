pipeline{

    agent any

    stages{

        stage('Build Jar'){
            steps{
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build Image'){
            steps{
                bat 'docker build -t=mrrajdoc/selenium:latest .'
            }
        }

        stage('Push Image'){
            environment{
                DOCKER_HUB = credentials('dockerhub-creds')
            }
            steps{
                bat 'echo ${DOCKER_HUB_PSW} | docker login -u ${DOCKER_HUB_USR} --password-stdin'
                bat 'docker push mrrajdoc/selenium:latest'
                bat "docker tag mrrajdoc/selenium:latest mrrajdoc/selenium:${env.BUILD_NUMBER}"
                bat "docker push mrrajdoc/selenium:${env.BUILD_NUMBER}"
            }
        }

    }

    post {
        always {
            bat 'docker logout'
        }
    }

}