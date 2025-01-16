pipeline {
    agent { label 'devops1-rmdhn'}
    tools { nodejs 'NodeJs' }
    

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/angkasatech/simple-apps.git'
            }
        }

        stage('Build') {
            steps {
                sh'''
                cd apps
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd apps
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd apps
                sonar-scanner \
                    -Dsonar.projectKey=Simple-Apps \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://172.23.10.14:9000 \
                    -Dsonar.login=sqp_99bb646e1faee6597a4bc23b1ee231fa5bb6af3f
                '''
            }
        }
        
        stage('Deploy Compose') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
    }
}