import groovy.json.JsonSlurperClassic

def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}

pipeline {
    
    agent any
    
    stages {
    
        stage("Paso 1: Download and checkout"){
    
            steps {
               checkout(
                        [$class: 'GitSCM',
                        branches: [[name: "pipeline-as-code" ]],
                        userRemoteConfigs: [[url: 'https://github.com/fernandogutierrez27/ejemplo-maven.git']]])
            }
    
        }
    
        stage("Paso 2: Compliar"){
    
            steps {
                script {
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean compile -e"
                }
            }
    
        }
    
        stage("Paso 3: Testear"){
    
            steps {
                script {
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean test -e"
                }
            }
    
        }
    
        stage("Paso 4: Build .Jar & publish artifact"){
    
            steps {
                script {
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                sh "mvn clean package -e"
                }
            }
            post {
                //record the test results and archive the jar file.
                success {
                    archiveArtifacts(artifacts:'build/*.jar', followSymlinks:false)
                }
            }
    
        }
    }
    
    post {
    
        always {
            sh "echo 'fase always executed post'"
        }

        success {
            sh "echo 'fase success!'"
        }

        failure {
            sh "echo 'fase failure'"
        }
    
    }
}