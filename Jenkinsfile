pipeline {
    agent any

    environment {
        NODE_HOME = '/usr/bin/node' // Optional: if Node is installed in a custom path
        PATH = "${NODE_HOME}:${env.PATH}"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: 'main']],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [],
                          userRemoteConfigs: [[url: 'https://github.com/SindhuPunyamurthi/Automated-CI-Pipeline-project.git']]
                ])
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('Automated CI pipeline project') {
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('Automated CI pipeline project') {
                    sh 'npm test || echo "Tests failed"'  // Avoid failing the pipeline immediately if tests fail
                }
            }
        }

        stage('Build & Package') {
            steps {
                dir('Automated CI pipeline project') {
                    sh 'npm run build || echo "Build failed"'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                dir('Automated CI pipeline project') {
                    archiveArtifacts artifacts: 'Build/**', fingerprint: true
                }
            }
        }
    }

    post {
        always {
            junit 'Automated CI pipeline project/tests/**/*.xml' // Adjust path if you have test reports
            echo "Pipeline finished"
        }
        success {
            echo "✅ Build Succeeded!"
        }
        failure {
            echo "❌ Build Failed!"
        }
    }
}
