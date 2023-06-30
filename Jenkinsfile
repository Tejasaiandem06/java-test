pipeline {
    agent any

    environment {
        function_name = 'sonarlambda'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn package'
            }
        }

        stages {
          stage("SonarQube analysis") {
            steps {
              withSonarQubeEnv('sonar') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }

        stage('Push') {
            steps {
                echo 'Push'

                sh "aws s3 cp target/sample-1.0.3.jar s3://sonartest12"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Build'
                sh "aws lambda update-function-code --function-name $function_name --region us-east-1 --s3-bucket redbull-f1 --s3-key sample-1.0.3.jar"
            }
        }
    }
}
