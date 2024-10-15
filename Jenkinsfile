pipeline{
    environment{
        DOCKERHUB_CRED = credentials("DockerSid")
    }
    agent any
    stages{

        stage("Stage 1 : Git Clone"){
            steps{
                git credentialsId: 'GitHubSid', url :"https://github.com/SiddharthChauhan303/Caluclator_SPE_MiniProject.git", branch: 'main'
            }
        }

        stage("Stage 2 : Maven Build"){
            steps{
                sh 'mvn clean install'
            }
        }

        stage("Stage 3 : Build Docker Image"){
            steps{
                sh "docker build -t siddharth303/calculator:latest ."
            }
        }

        stage("Stage 4 : Push Docker Image to Dockerhub"){
            steps{
                sh 'echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin'
                sh "docker push siddharth303/calculator:latest"
            }
        }

        stage("Stage 5 : Clean Unwanted Docker Images"){
            steps{
                sh "docker container prune -f"
                sh "docker image prune -a -f"
            }
        }

        stage('Stage 6 : Ansible Deployment') {
            steps {
                    sh 'sudo ansible-playbook -i inventory Deploy_Calculator.yml'
             }
        }
    }
}