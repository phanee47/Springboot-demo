node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('mvn build') {
        sh 'mvn clean package'
    }

    stage('Build image') {
  
       app = docker.build("phanee47/springtest")
    }


    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
}