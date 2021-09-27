pipeline {
    agent {label "slavenode1"}
    
    triggers {
        pollSCM '* * * * *'
    }

    
    stages {
        
        /* ----------- Development Stages ---------- */
        stage('Pull Dev Code') {
            when { branch 'dev' }
            steps {
                git branch: 'dev', url: 'https://github.com/mypractice96/angular-employee-app.git'
            }
        }
        
        stage('Package - Dev') {
            when { branch 'dev' }
            steps {
                sh 'npm install'
            }
        }
        
        stage('Docker Build - Dev') {
            when { branch 'dev' }
            steps {
                sh 'ng build'
                sh 'docker build -t employee-dev:${BUILD_NUMBER} .'
            }
        }
        
        stage('Deploy to Dev') {
            when { branch 'dev' }
            steps {
               sh 'docker rm -f employee-dev || true'
               sh 'docker run -d --name employee-dev -p 7011:80 employee-dev:${BUILD_NUMBER}'
            }
        }
        
        
        
        
        /* ---------- QA Stages ----------*/
        stage('Pull QA Code') {
            when { branch 'qa' }
            steps {
                git branch: 'qa', url: 'https://github.com/mypractice96/angular-employee-app.git'
            }
        }
        stage('QA Package') {
            when { branch 'qa' }
            steps {
                sh 'npm install'
            }
        }
        stage('Docker Build - QA') {
            when { branch 'qa' }
            steps {
                sh 'ng build'
                sh 'docker build -t employee-qa:${BUILD_NUMBER} .'
            }
        }
        
        stage('Deploy to QA') {
            when { branch 'qa' }
            steps {
               sh 'docker rm -f employee-qa || true'
               sh 'docker run -d --name employee-qa -p 7012:80 employee-qa:${BUILD_NUMBER}'
            }
        }
        
        /* ---------- Prod Stages ----------*/
        stage('Pull Code') {
            when { branch 'main' }
            steps {
                git branch: 'main', url: 'https://github.com/mypractice96/angular-employee-app.git'
            }
        }
        stage('Package') {
            when { branch 'main' }
            steps {
                sh '/opt/node-12/bin/npm install'
            }
        }
        stage('Docker Build') {
            when { branch 'main' }
            steps {
                sh '/opt/node-12/bin/ng build'
                sh 'docker build -t employee:${BUILD_NUMBER} .'
            }
        }
        
        stage('Deploy') {
            when { branch 'main' }
            steps {
               sh 'docker rm -f employee || true'
               sh 'docker run -d --name employee -p 7013:80 employee:${BUILD_NUMBER}'
            }
        }
        
    
        
    }
}
