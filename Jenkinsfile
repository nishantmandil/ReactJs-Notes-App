pipeline {
    agent { label 'Jenkins-Agent' }

    environment {
        // Limit Node.js memory usage to 512 MB
        NODE_OPTIONS = "--max_old_space_size=512"
        // Tell React to run in CI mode (no interactive prompts/watchers)
        CI = "true"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main',
                    credentialsId: 'github',
                    url: 'https://github.com/nishantmandil/ReactJs-Notes-App.git'
            }
        }

        stage("Install Dependencies") {
            steps {
                sh "npm install --no-audit --no-fund"
            }
        }

        stage("Build Application") {
            steps {
                sh "npm run build"
            }
        }

        stage("Test Application") {
            steps {
                sh "npm test -- --watchAll=false"
            }
        }

        stage("Archive Build") {
            steps {
                archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
            }
        }
    }
}
