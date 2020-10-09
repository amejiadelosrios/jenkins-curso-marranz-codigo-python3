pipeline {
    agent any

    stages {
        stage('Checkout-git') {
            steps {
                git poll: true, url: 'git@github.com:amejiadelosrios/test-jerkins-1.git'
            }
        }
        stage('CreateVirtualEnv') {
            steps {
                    sh '''
                            bash -c "virtualenv entorno_virtual && source entorno_virtual/bin/activate"
                    '''
            }
        }
        stage('InstallRequirements') {
            steps {
                    sh '''
                            bash -c "source /var/jenkins_home/workspace/test-jenkins-1/entorno_virtual/bin/activate && /var/jenkins_home/workspace/test-jenkins-1/entorno_virtual/bin/python3 /var/jenkins_home/workspace/test-jenkins-1/entorno_virtual/bin/pip3 install -r requirements.txt"
                    '''
            }
        }
        stage('TestApp') {
            steps {
                    sh '''
                        bash -c "source /var/jenkins_home/workspace/test-jenkins-1/entorno_virtual/bin/activate &&  cd src && /var/jenkins_home/workspace/test-jenkins-1/entorno_virtual/bin/python3 /var/jenkins_home/workspace/test-jenkins-1/entorno_virtual/bin/pytest && cd .."
                    '''
            }
        } 
        stage('RunApp') {
            steps {
                    sh '''
                        bash -c "source entorno_virtual/bin/activate ; /var/jenkins_home/workspace/test-jenkins-1/entorno_virtual/bin/python3 src/main.py &"
                    '''
            }
        } 
        stage('BuildDocker') {
            steps {
                    sh '''
                        docker build -t apptest:latest .
                    '''
            }
        } 
    stage('PushDockerImage') {
            steps {
                    sh '''
                        docker tag apptest:latest amejiadelosrios/apptest:latest
                                        docker push amejiadelosrios/apptest:latest
                                        docker rmi -f apptest:latest
                    '''
            }
        } 
    }
}