pipeline{
    agent any
    tools{
        maven "maven"
    }
    environment{
        registry = "monjoychoudhury29/calculator_demo"
        registryCredential = "dockerhub"
        dockerImage = ""
        BUILD_NUMBER = "1.0"
    }
    stages{
        stage('Pull GitHub'){
            steps{
                git branch: 'master',  url: 'https://github.com/MnCSSJ4x/DevOps-Calculator.git'
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
                    docker.withRegistry('', registryCredential){
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
         stage('Ansible Deploy'){
        steps{
                ansiblePlaybook colorized: true,
                disableHostKeyChecking: true,
                installation: 'Ansible',
                inventory: 'inventory',
                playbook: 'playbook.yml'
            }
        }
    }
}
