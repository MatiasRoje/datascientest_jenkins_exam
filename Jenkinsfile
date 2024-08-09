pipeline {
    environment {
        imagename = 'matiasroje/root-service'
        registryCredential = 'dockerhub'
        dockerImage = ''
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
                    branch: 'master',
                    credentialsId: 'github'
                )
            }
        }

        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build(imagename, "root-service")
                }
            }
        }

        stage('Test image') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'echo "Here some testing should be implemented"'
                    }
                }
            }
        }

        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Trigger ManifestUpdate') {
            steps {
                script {
                    echo "Triggering the Update Manifest Job"
                    build job: 'Update Manifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
                }
            }
        }
    }
}
