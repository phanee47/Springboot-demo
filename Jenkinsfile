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
    
    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email phanee47@gmail.com"
                        sh "git config user.name Phanee"
                        //sh "git switch master"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+phanee47/springtest.*+phanee47/springtest:${env.BUILD_NUMBER}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/Springboot-demo.git HEAD:main"
      }
    }
  }

}

    stage ('run deploy') {
        withKubeConfig([credentialsId: 'kubeconfig']){
        sh 'kubectl apply -f deployment.yaml'
        }
    }

}