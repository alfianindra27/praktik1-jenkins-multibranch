pipeline {
    agent {
        docker {
            image 'python:3.10'
        }
    }

    stages{
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }
        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comprator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST'
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload)
                    url: 'https://discord.com/api/webhooks/1425353703581552741/YElLcdan0aCqoY2V3lxsI9L7x6dgc7ddnpTHq51txjGT4Vj7gTZgprxWIkHn7OU7jFwZ'
                )
            }
        }
        failure {
            script {
                def payload = [
                    content: "❌ Build FAILED on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST'
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload)
                    url: 'https://discord.com/api/webhooks/1425353703581552741/YElLcdan0aCqoY2V3lxsI9L7x6dgc7ddnpTHq51txjGT4Vj7gTZgprxWIkHn7OU7jFwZ'
                )
            }
        }
    }
}
