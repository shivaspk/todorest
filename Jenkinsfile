pipeline {
    agent {
        docker {
            image 'adoptopenjdk/maven-openjdk11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'java -version'
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh 'mvn install'
            }
        }
        stage('Docker build')
        {
            steps{
                sh 'docker build -t shivaspk/todorestcicd .'
            }
        }
        stage('Docker push')
        {
            steps{
                   script{
                    docker.withRegistry('https://registry.hub.docker.com/', 'docker-shivaspk') {
                        docker.push('shivaspk/todocicd:latest')
                    }
                }
            }
        }
    }
}