pipeline {
    agent any
   
    stages {
        stage('Setup Python') {
    steps {
        bat '''
        curl -o python-installer.exe https://www.python.org/ftp/python/3.11.5/python-3.11.5-amd64.exe
        start /wait python-installer.exe /quiet InstallAllUsers=1 PrependPath=1
        '''
    }
}


        stage('Run Selenium Tests with pytest') {
            steps {
                    echo "Running Selenium Tests using pytest"

                    // Install Python dependencies
                    bat 'pip install -r requirements.txt'

                    //  Start Flask app in background
                    bat 'start /B python app.py'

                    // Wait a few seconds for the server to start
                    bat 'ping 127.0.0.1 -n 5 > nul'

                    // Run tests using pytest
                    
                    bat 'python -m pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t seleniumdemoapp:v1 ."
            }
        }
        stage('Docker Login') {
            steps {
                  bat 'docker login -u 22251a1257it258 -p Manogna18@gnits'
                }
            }
        stage('push Docker Image to Docker Hub') {
            steps {
                echo "push Docker Image to Docker Hub"
                bat "docker tag seleniumdemoapp:v1 22251a1257it258/sample1:seleniumtestimage"               
                    
                bat "docker push 22251a1257it258/sample1:seleniumtestimage"
                
            }
        }
        stage('Deploy to Kubernetes') { 
            steps { 
                    // apply deployment & service 
                    bat 'kubectl apply -f deployment.yaml --validate=false' 
                    bat 'kubectl apply -f service.yaml' 
            } 
        }
        
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
