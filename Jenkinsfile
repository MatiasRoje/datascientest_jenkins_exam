pipeline {
    environment {
        registryCredential = 'dockerhub'
        rootName = 'matiasroje/root-service-dev'
        movieName = 'matiasroje/movie-service-dev'
        castName = 'matiasroje/cast-service-dev'
        rootImage = ''
        movieImage = ''
        castImage = ''
    }

    agent any

    stages {
        stage('Verify Docker') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Cloning Git') {
            steps {
                git(
                    url: 'https://github.com/MatiasRoje/datascientest_jenkins_exam.git',
                    branch: 'dev',
                    credentialsId: 'github'
                )
            }
        }

        stage('Testing code') {
            steps {
                sh 'echo "Here some testing could be implemented"'
            }
        }

        stage('Building images') {
            steps {
                script {
                    rootImage = docker.build(rootName, "-f root-service/Dockerfile root-service")
                    movieImage = docker.build(movieName, "-f movie-service/Dockerfile movie-service")
                    castImage = docker.build(castName, "-f cast-service/Dockerfile cast-service")
                }
            }
        }

        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        rootImage.push("$BUILD_NUMBER")
                        rootImage.push('latest')
                        movieImage.push("$BUILD_NUMBER")
                        movieImage.push('latest')
                        castImage.push("$BUILD_NUMBER")
                        castImage.push('latest')
                    }
                }
            }
        }

        stage('Trigger ManifestUpdate') {
            steps {
                script {
                    echo "Triggering the Update Manifest Job"
                    build job: 'Update Manifest Dev', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
                }
            }
        }
    }
}
