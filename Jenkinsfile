@Library("shared") _
pipeline{
    agent {label "Node1"}
stages{
    stage("Hello"){
    steps{
        script{
            hello()
        }
    }
   }
    stage("Code"){
        steps{
            script {
            clone ("https://github.com/Harshal77777/django-notes-app.git","main")
            }
           echo "code cloned "
        }
    }
    stage("Build"){
        steps{
            echo "building the code"
            sh "docker build -t notes-app:latest ."
        }
    }
    stage("Push to DockerHub"){
        steps{
            echo "Pushing image to DH"
            withCredentials([usernamePassword
            (credentialsId:"dockhubcred",
            passwordVariable:"dockpass",
            usernameVariable:"dockuse")]){
             
             sh '''
            echo "$dockpass" | docker login -u "$dockuse" --password-stdin
            docker image tag notes-app:latest $dockuse/notes-app:latest
            docker push $dockuse/notes-app:latest
   
            '''
                
            }
        }
    }
    stage("Deploy"){
        steps{
            echo "Deploying the code"
            sh "docker compose up -d"
        }
    }
}    
}
