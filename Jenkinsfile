node {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        git 'https://github.com/hTobi02/docker-tvheadend-oscam-icam.git'
    }
    
    stage('Login Docker') {
        sh 'echo jenkins | docker login repo.minenbauer.de -u jenkins --password-stdin'
    }

    stage('Build image') {
        sh 'docker buildx build --platform=linux/amd64 -t htobi02/tvheadend-oscam-icam:amd64 .'
        sh 'docker buildx build --platform=linux/arm64 -t htobi02/tvheadend-oscam-icam:arm64 .'
    }

    stage('Create Manifest') {
        sh 'docker manifest create htobi02/tvheadend-oscam-icam:latest \
                --amend htobi02/tvheadend-oscam-icam:amd64 \
                --amend htobi02/tvheadend-oscam-icam:arm64'
    }

    stage('Push image') {
        sh 'docker push htobi02/tvheadend-oscam-icam:latest'
    }
    
    stage('Logout Docker') {
            sh 'docker logout'
    }
}

docker buildx create --name multiarch --driver docker-container --bootstrap --use