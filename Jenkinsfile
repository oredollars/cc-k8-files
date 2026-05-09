pipeline {
    agent any

    parameters {
        string(
            name: 'DOCKER_TAG',
            defaultValue: 'latest',
            description: 'Docker image tag'
        )
    }

    stages {

        stage('Cleanup workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main',
                credentialsId: 'github',
                url: 'https://github.com/oredollars/cc-k8-files.git'
            }
        }

        stage('Update GIT') {
            steps {
                script {

                    withCredentials([
                        usernamePassword(
                            credentialsId: 'githubcred',
                            passwordVariable: 'GIT_PASSWORD',
                            usernameVariable: 'GIT_USERNAME'
                        )
                    ]) {

                        sh """
                        git config user.email "daiyepeku@gmail.com"
                        git config user.name "daniel"

                        sed -i 's+oredollar/color-checker-service:.*+oredollar/color-checker-service:${DOCKER_TAG}+g' deployment.yaml

                        git add .

                        git commit -m "Updated image to ${DOCKER_TAG}" || echo "No changes to commit"

                        git remote set-url origin https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/oredollars/cc-k8-files.git

                        git push origin main
                        """
                    }
                }
            }
        }
    }
}
