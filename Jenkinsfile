import groovy.json.JsonSlurperClassic
def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage("Paso 1: Compliar"){
            steps {
                script {
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean compile -e"
                }
            }
        }
        stage("Paso 2: Testear"){
            steps {
                script {
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean test -e"
                }
            }
        }
        stage("Paso 3: SonarQube Analysis"){
            steps {
                withSonarQubeEnv('SonarQubeUsach') {
                    sh "echo 'SonarQube Analysis!'"
                    sh "echo ${env.SONAR_HOST_URL}"
                    sh "echo ${env.SONAR_CONFIG_NAME}"
                    // Run Maven on a Unix agent.
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar -Dsonar.projectKey=test-alive'
                }
            }
        }
        stage("Paso 4: Build .Jar"){
            steps {
                script {
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                sh "mvn clean package -e"
                }

                sh "echo 'Bye World'"
            }
            post {
                //record the test results and archive the jar file.
                success {
                    archiveArtifacts artifacts:'build/*.jar'
                }
            }
        }
    }
    post {
        always {
            sh "echo 'fase always executed post'"
        }
        success {
            sh "echo 'fase success'"
        }
        failure {
            sh "echo 'fase failure'"
        }
    }
}