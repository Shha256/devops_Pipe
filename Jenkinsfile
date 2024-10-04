// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: python
    image: python:3.9.12-alpine3.15
    command:
    - sleep
    args:
    - infinity
  hostAliases:
  - ip: "127.21.0.50"
    hostnames:
    - "gitea.localhost.com"

'''
         
          //  defaultContainer 'shell'
          //  retries 2
        }
    }
}
    stages {
        stage('Unit teste') {
            container('python')
            steps {
                sh '''
                pip install -r requirements.txt
                bandit -r . -x '/.venv/','/tests/'
                black .
                flake8 . --exclude .venv
                pytest -v --disable-warnings
                ,,,

        }
    }
}