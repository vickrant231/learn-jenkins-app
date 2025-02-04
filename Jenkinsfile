pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID='d4534b12-28a3-4365-af8c-929ccd2db976';
    }

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                ls -la
                node --version
                npm --version
                npm ci
                npm run build                
                '''
            }
        }
        stage("Test"){
             agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }

         stage('Deploy') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''               
                npm install netlify-cli
                node_modules/.bin/netlify --version
                echo "Deploying to production.Site ID: $NETLIFY_SITE_ID"  
                '''
            }
        }
    }
    
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
    


}
