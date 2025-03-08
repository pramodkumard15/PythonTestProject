pipeline {
    agent any
    
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/pramodkumard15/PythonTestProject.git'
            }
        }
        
        stage('Run Python Script') {
            steps {
                sh 'python3 script.py'
            }
        }
    }
}
