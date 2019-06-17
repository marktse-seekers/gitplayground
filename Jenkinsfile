node('docker'){
    def prNumber = null

    git branch: 'dev', changelog: false, credentialsId: 'GIT-USER-CRED', poll: false, url: 'https://github.com/SeekersAdvisorsLabs/aqt-web'
    withCredentials([usernamePassword(credentialsId: 'GIT-USER-CRED', passwordVariable: 'GIT_PWD', usernameVariable: 'GIT_USER')]) {
        
        stage("Automated Merge"){
            withCredentials([usernamePassword(credentialsId: 'GIT-ADMIN-CRED', passwordVariable: 'GIT_ADMINPW', usernameVariable: 'GIT_ADMIN')]) {
                def cmd = "curl -X PUT -i -u ${GIT_ADMIN}:'${GIT_ADMINPW}' https://api.github.com/repos/SeekersAdvisorsLabs/aqt-web/pulls/3/merge"
                echo "${cmd}"
                sh "docker rm -f release | true"
                sh "docker run --rm -di --entrypoint /bin/bash --name release jenkins_jenkins-deployment-agent:latest"
                sh "docker cp /opt/git/gitadminconfig release:/root/.gitconfig"
                sh "docker exec release sh -c '${cmd}'"
                sh "docker stop release"
            }    
        }   
    }
}
