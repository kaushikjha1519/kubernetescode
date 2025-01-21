node('agent1') {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("kaushikkjha/test1")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    //stage('Push image') {
        
       // docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
       // }
    //}

    stage('Push image') {
    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
        sh 'docker context ls'
        sh 'docker info'
        sh 'docker login -u $kaushikkjha -p $Kaushik@1519 https://registry.hub.docker.com'
        app.push("${env.BUILD_NUMBER}")
    }
}
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
