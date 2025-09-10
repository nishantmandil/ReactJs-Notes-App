pipeline {
    agent { label 'Jenkins-Agent' }

    environment {
        NODEJS_HOME = tool name: 'NodeJS16', type: 'NodeJS'
        PATH = "${env.NODEJS_HOME}/bin:${env.PATH}"
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds() // ensures only 1 build runs at a time
        timestamps()              // adds timestamps to console logs
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
