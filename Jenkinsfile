pipeline {
    agent { label 'Jenkins-Agent' }

    stages {
        stage("Cleanup Workspace") {
            steps { cleanWs() }
        }
        stage("Checkout from SCM") {
            steps { git branch: 'main', credentialsId: 'github', url: 'https://github.com/nishantmandil/ReactJs-Notes-App.git' }
        }
        stage("Install Dependencies") {
            steps { sh "npm install" }
        }
        stage("Build Application") {
            steps { sh "npm run build" }
        }
        stage("Test Application") {
            steps { sh "npm test -- --watchAll=false" }
        }
        stage("Archive Build") {
            steps { archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true }
        }
    }
}
