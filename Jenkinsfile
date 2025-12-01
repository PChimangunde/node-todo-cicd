@Library('my_library')

pipeline {
    agent { label 'mynote-app' }
    stages {
        stage ('code'){
            steps{
               echo "this stage clone/pull code from github" 
                clone ( "https://github.com/PChimangunde/node-todo-cicd.git", "master" )
            }
        }
        stage ('build and test'){
            steps{
                echo "this stage build the images using docker build"
                sh "docker build -t node-app:latest ."
            }
        }
        stage ('dockerhub push'){
            steps{
                echo "this stage will push image to dockerhub repo"
                
                withCredentials([usernamePassword(
                                 credentialsId: 'dockerhubCreds',
                                 usernameVariable: 'Username',
                                 passwordVariable: 'passv'
                                 )]) {

                    sh """
                         echo "$passv" | docker login -u "$Username" --password-stdin

                         docker image tag node-app:latest $Username/node-app:latest

                         docker push $Username/node-app:latest
                       """
                     }
                
            }
            
        }
        stage ('deploy'){
            steps{
             echo "this stage deploy the image using docker compose"  
             sh "docker compose down && docker compose up -d --build"

                echo "print deployment is success"
            }
        }
    }
    
    
}
