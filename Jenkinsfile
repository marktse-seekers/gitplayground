node('docker'){
    def prNumber = null
    withCredentials([usernamePassword(credentialsId: 'GIT-USER-CRED', passwordVariable: 'GIT_PWD', usernameVariable: 'GIT_USER')]) {
        
        stage("Automated Merge"){
            withCredentials([usernamePassword(credentialsId: 'GIT-ADMIN-CRED', passwordVariable: 'GIT_ADMINPW', usernameVariable: 'GIT_ADMIN')]) {
                def cmd = "curl -X PUT -i -u ${GIT_ADMIN}:'${GIT_ADMINPW}' https://api.github.com/repos/'marktse-seekers'/gitplayground/pull/3/merge"
                echo "${cmd}"
                sh "docker rm -f release || true"
                sh "docker run --rm -di --entrypoint /bin/bash --name release -v /opt/git/gitadminconfig:/root/.gitconfig jenkins_jenkins-deployment-agent:latest"
                sh "docker exec release sh -c '${cmd}'"
                sh "docker exec release sh -c 'cat /root/.gitconfig'"
                sh "docker stop release"
            }    
        }   
    }
}
