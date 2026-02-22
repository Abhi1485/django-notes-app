pipeline {
    agent { label 'infra' }

    stages {
        stage('Code') {
            steps {
                echo 'This is cloning the code'
                git url:'https://github.com/Abhi1485/django-notes-app.git', branch:'main' 
                echo 'The code cloning is successful'
            }
        }

        stage('Build') {
            steps {
                echo 'This is building the code'
                sh 'whoami'
                sh 'docker build -t notes-app:latest .'
            }
        }

        stage('Push to DockerHub') {
    steps {
        echo 'This is pushing the image to DockerHub'
        withCredentials([usernamePassword(credentialsId: 'DockerHubCred',
                                          usernameVariable: 'DockerHubUser',
                                          passwordVariable: 'DockerHubPass')]) {
            sh '''
                docker login -u $DockerHubUser -p $DockerHubPass
                docker image tag notes-app:latest $DockerHubUser/notes-app:latest
                docker push $DockerHubUser/notes-app:latest
            '''
        }
    }
}


        stage('Deploy') {
            steps {
                echo 'This is deploying the code'
                sh 'docker compose up -d'
            }
        }
    }
}

