pipeline{
    agent any
    stages{
        stage("Clean Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Git: Clone Repository"){
            steps{
                git url "", branch: "main"
            }
        }
        stage("Trivy: Filesystem Scan"){
            steps{
                echo "scaned!...."
            }
        }
        stage("OWASP: Dependency check"){
            steps{
                echo "OWASP!...."
                // script{
                //     owasp_dependency()
                // }
            }
        }
        
        stage("SonarQube: Code Analysis"){
            steps{
                echo "SonarQube!...."
                // script{
                //     sonarqube_analysis("Sonar","bankapp","bankapp")
                // }
            }
        }
        
        stage("SonarQube: Code Quality Gates"){
            steps{
                echo "SonarQube!...."
                // script{
                //     sonarqube_code_quality()
                // }
            }
        }
         stage("Docker: Build Image") {
            steps {
                sh "docker build -t bannkapp ." 
            }
        }
        stage("DockerHub: Push Image") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "DOCKER_USERNAME",
                    passwordVariable: "DOCKER_PASSWORD"
                )]) {
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    sh "docker tag bannkapp:latest ${DOCKER_USERNAME}/bannkapp:latest" 
                    sh "docker push ${DOCKER_USERNAME}/bannkapp:latest"  
                }
            }
        }

    }
    post{
        success{
            echo "success pushed"
            // archiveArtifacts artifacts: '*.xml', followSymlinks: false
            // build job: "BankApp-CD", parameters: [
            //     string(name: 'DOCKER_TAG', value: "${params.DOCKER_TAG}")
            // ]
        }
    }
}