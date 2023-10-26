pipeline{
    tools {
        maven 'maven-3.6.3'
    }
    agent any

    environment{
        registry = ""
        registryCredential = ""
        dockerImage = ""
        BUILD_NUMBER = "1.0.0"
    }
    stages{
        stage('Pull GitHub'){
            steps{
                git branch: 'master',  url: ''
            }
        }
        stage('Build Maven Jar'){
            steps{
                    sh 'mvn clean install'
            }
        }
        stage('Docker Image Build'){
            steps{
                script{
                    dockerImage = docker.build(registry + ":$BUILD_NUMBER")
                }
            }
        }
        stage('Docker Image Push'){
            steps{
                script{
                    docker.withRegistry( '', registryCredential){
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Clean Up'){
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
    stage('Ansible Deploy'){
        steps{
            ansiblePlaybook colorized: true,
            disableHostKeyChecking: true,
            installation: 'ansible',
            inventory: 'inventory',
            playbook: 'playbook.yml'
        }
    }

}
