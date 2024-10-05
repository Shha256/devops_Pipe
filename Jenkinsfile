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
        }
    }
    
    stages {
        stage('Unit teste') {
            when {
                anyOf {
                    branch pattern: "feature-*", comparator: "GLOB"
                    branch pattern: "develop", comparator: "GLOB"
                    branch pattern: "hotfix", comparator: "GLOB"
                    branch pattern: "release-*", comparator: "GLOB"
                    branch pattern: "v*", comparator: "GLOB"
                }
            }
            steps {
                container('python') {
                    sh '''
                    pip install -r app/requirements.txt
                    bandit -r . -x '/.venv/','/tests/'
                    black .
                    flake8 . --exclude .venv --ignore=E501
                    pytest -v --disable-warnings
                    '''
                }
            }
        }
    }
}
