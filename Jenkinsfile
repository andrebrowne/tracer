node {
    checkout scm 
}

pipeline {
    agent any
    tools {
        maven "M3"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                echo 'Build complete!'
            }
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh "mvn test"
                echo 'Testing complete!'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying..'
                azureWebAppPublish appName: 'boomerang-dev', azureCredentialsId: 'azure_service_principal', dockerImageName: '', dockerImageTag: '', dockerRegistryEndpoint: [], filePath: '', publishType: 'file', resourceGroup: 'CICD', slotName: 'dev', sourceDirectory: '', targetDirectory: ''
                echo 'Deployment complete!'
            }
        }
    }
}
