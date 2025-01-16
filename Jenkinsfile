pipeline {
    agent { label 'devops1-rmdhn'}
    tools { nodejs 'NodeJs' }
    

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/IDN-Training/simple-apps.git'
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
                cd app
                sonar-scanner \
                    -Dsonar.projectKey=Simple-Apps \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://172.23.10.14:9000 \
                    -Dsonar.login=sqp_6162fa52b4e2f2082ac6c21a8106b0a0cf1ee3db
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
        
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}