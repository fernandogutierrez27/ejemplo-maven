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
                    // sh "echo ${env.SONAR_HOST_URL}"  #-> Just for testing - Removed
                    // sh "echo ${env.SONAR_CONFIG_NAME}"  #-> Just for testing - Removed
                    // Run Maven on a Unix agent.
                    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=test-alive'
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
         stage("Paso 5: Levantar Springboot APP"){
            steps {
                sh 'mvn spring-boot:run &'
            }
        }
        stage("Paso 6: Dormir(Esperar 10sg) "){
            steps {
                sh 'sleep 10'
            }
        }
        stage("Paso 7: Test Alive Service - Testing Application!"){
            steps {
                sh 'curl -X GET "http://localhost:8081/rest/mscovid/test?msg=testing"'
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