pipeline {
    agent { label 'production' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', credentialsId: 'auth-github', url: 'https://github.com/pandeprana/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd app
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd app
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd app
                sonar-scanner   -Dsonar.projectKey=simple-apps   -Dsonar.sources=.   -Dsonar.host.url=http://172.23.6.126:9000   -Dsonar.token=sqp_150d9be75a2776347e7984b11a6a23c149bfc723
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        // stage('Backup') {
        //     steps {
        //          sh 'docker compose push' 
        //     }
        // }
    }
}