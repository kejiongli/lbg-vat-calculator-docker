pipeline{
 environment {
 registry = "innovationami/vatcal"
        registryCredentials = "dockerhub_id"
        dockerImage = ""
    }
    agent any
        stages {
            stage ('Build Docker Image'){
                steps{
                    script {
                        dockerImage = docker.build(registry)
                    }
                }
            }

            stage ("Push to Docker Hub"){
                steps {
                    script {
                        docker.withRegistry('', registryCredentials) {
                            dockerImage.push("${env.BUILD_NUMBER}")
                            dockerImage.push("latest")
                        }
                    }
                }
            }

            stage ("Clean up"){
                steps {
                    script {
                        sh 'docker image prune --all --force --filter "until=48h"'
                           }
                }
            }

            stage('SonarQube Analysis') {
                environment {
                    scannerHome = tool 'sonarqube'
                    }
                steps {
                    withSonarQubeEnv('sonar-qube-1') {        
                    sh "${scannerHome}/bin/sonar-scanner"
                    }
                // timeout(time: 10, unit: 'MINUTES'){
                // waitForQualityGate abortPipeline: true
                    }
                 }
            }

}
