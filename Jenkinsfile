pipeline {
    agent any
    
    stages {
        stage ('Fetch code '){
            steps {
                git branch: 'main', url: 'https://github.com/maisaarig/test_pipeline'
            }
        }
        stage ('Build '){
            steps {
                bat 'mvn clean install -DeskipTests'
            }
            post{
                success {
                    echo "Now archiving. Build stage"
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('UNIT TEST') {
            steps{
                bat 'mvn test'
            }
        }

        stage('Checkstyle ANALYSIS'){  //added this
            steps {
                bat 'mvn checkstyle:checkstyle'
            }
        }
        stage ('Sonar Analysis'){
            environment {
                scannerHome = tool 'sonarqube'
            }
            steps {
                withSonarQubeEnv('sonaqube') { // If you have configured more than one global server connection, you can specify its name
                bat '''%scannerHome%/bin/sonar-scanner -Dsonar.projectKey=test_pipeline \
                -Dsonar.projectName=test_pipeline \
                -Dsonar.projectVersion=1.0 \
                -Dsonar.sources=src/ \
                -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                -Dsonar.junit.reportPath=target/surefire-reports/ \
                -Dsonar.jacoco.reportPath=target/jacoco.exec \
                -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }
        }
    }
}
