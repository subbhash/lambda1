pipeline {
    agent {
    label 'test'
  }

    stages {
        stage("Checkout") {
            steps {
              
            }
        }
        stage("Build") {
            steps {
                sh "python3 -m venv venv"
                sh ". venv/bin/activate"
                sh "pip install -r requirements.txt"
            }
        }

        stage("Test") {
            steps {
                sh "python -m pytest tests/"
            }
        }

        stage("Package") {
            steps {
                sh "zip -r function.zip ."
                archiveArtifacts artifacts: 'function.zip', fingerprint: true
            }
        }

        stage("Deploy") {
            steps {
                step([$class: 'LambdaCreateFunctionBuilder', credentialsId: 'aws-credentials', region: 'ap-southeast-2', functionName: 'my-function', runtime: 'python3.9', artifact: 'function.zip', handler: 'index.handler', role: 'arn:aws:iam::456464382094:role/lambda-role'])
            }
        }
    }
}
