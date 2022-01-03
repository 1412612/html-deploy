pipeline {

  agent any
  environment {
    DOCKER_IMAGE = "1412612/html-deploy"
  }

  stages {
      
    stage("build") {
            
        steps {
        
        withDockerRegistry(credentialsId: 'dockerhub-1412612', url: 'https://index.docker.io/v1/') {
            
            sh "docker build -t ${DOCKER_IMAGE} . "
            sh "docker push ${DOCKER_IMAGE}"
        }    

            //clean to save disk
            sh "docker image rm -f ${DOCKER_IMAGE}"
            sh "docker image prune -f"
        }

    }
	  
    stage("ssh"){
            
        steps {
                
        sshPublisher(publishers: [sshPublisherDesc(configName: 'server-1412612', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: """cd ~
            docker stop html-deploy-container
            docker rm -f html-deploy-container
            docker image rm -f 1412612/html-deploy
            docker run -p 80:80 --name html-deploy-container -d 1412612/html-deploy
            docker image prune -f""", execTimeout: 120000000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        }
    } 

  }

}