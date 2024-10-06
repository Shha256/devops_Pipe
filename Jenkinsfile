pipeline {
    agent {
        kubernetes {

            yamlFile 'jenkinspod.yaml'


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
