pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        
        stage('Build Docker Image') {
            when {
               branch 'master'
            }
            steps {
                script {
                  app = docker.build("srununna/train-schedule")
                    app.inside { 
                      sh 'echo $(curl localhost:8080)'
                    }  
                }
            }
        }
            
            stage('Push Docker Image') {
            when {
               branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }      
            }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                milestone(1)
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernamevariable: 'USERNAME', passwordVariable: 'USERPASS')])
                script {
                     sh "sshpass -p '$USERPASS' -v ssh -o StrictHostkeyChecking=no $USERNAME@$prod_ip \"docker pull srununna/train-schedule:
            }
        
        }
    } 
}
