pipeline {
    agent { label 'Jenkins-Agent' }

    environment {
        NODEJS_HOME = tool name: 'NodeJS16', type: 'NodeJS'
        PATH = "${env.NODEJS_HOME}/bin:${env.PATH}"
    }

    options {
        // Discard old builds to save disk space
        buildDiscarder(logRotator(numToKeepStr: '10'))
        // Limit concurrent builds on this agent to 1
        throttleConcurrentBuilds(throttleEnabled: true, maxConcurrentPerNode: 1)
    }

    stages {

        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/nishantmandil/ReactJs-Notes-App.git'
            }
        }

        stage("Install Dependencies") {
            steps {
                script {
                    // Use npm ci for clean and faster installs
                    sh '''
                        if [ -d "node_modules" ]; then
                            echo "node_modules exists, skipping reinstall"
                        else
                            npm ci --no-audit --no-fund
                        fi
                    '''
                }
            }
        }

        stage("Build Application") {
            steps {
                sh 'npm run build'
            }
        }

        stage("Test Application") {
            steps {
                sh 'npm test -- --watchAll=false'
            }
        }

        stage("Archive Build") {
            steps {
                archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo "Cleaning temporary files to free memory"
            sh 'rm -rf tmp/* || true'
        }
        success {
            echo "Build completed successfully!"
        }
        failure {
            echo "Build failed. Check logs."
        }
    }
}
