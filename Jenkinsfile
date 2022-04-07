pipeline{
    environment {
        imagename = "abeerab/imagef"
        registryCredential = "dockerhub_credentials"
        // dockerImage = ''
        def scannerHome = tool 'SonarScanner'
    }
    agent any
    stages{
        stage("test-sonar"){
            steps{
                script {
                    withSonarQubeEnv("SonarQube") {
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=mern-frontend \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=admin \
                        -Dsonar.password=123456789"
                    } 
                }
            }
        }
        stage("build"){
            
            steps{
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage("docker-build"){
            steps{
                script {
                    dockerImage = docker.build imagename   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                    }
                }
            }
        }
        stage("deploy"){
            steps{
                echo 'deployment'
            }
        }
    }
}
