pipeline {
    agent any
    
    tools {
        maven 'maven3'
        jdk 'jdk8'
    }
    parameters {
        string(name: 'tomcat_dev', defaultValue: '18.130.77.126', description: 'Staging Server')
        string(name: 'tomcat_prd', defaultValue: '18.130.77.126', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')
    }


    stages {
        stage('Install') {
            steps {
                sh "mvn -U clean test cobertura:cobertura -Dcobertura.report.format=xml"
            }
            post {
                always {
                    junit '**/target/*-reports/TEST-*.xml'
                }
            }
        }
    }
    
    /*stages {
        
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                always {
                    junit '**/target/*-reports/TEST-*.xml'
                }
                success {
                    echo 'Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        } */

        stage('Deployment'){
            parallel {
                stage('Deploy to Staging'){
                    steps {
                        sh "echo 'Depploy to Staging'"
                        //sh "scp -i /var/lib/jenkins/hieutr.pem -o StrictHostKeyChecking=no **/target/*.war ubuntu@${params.tomcat_dev}:~/apache-tomcat-8.5.28-stagg/webapps"
                    }
                }
                stage('Deploy to Production'){
                    steps {
                        sh "echo 'Deploy to Production'"
                        //sh "scp -i /var/lib/jenkins/hieutr.pem -o StrictHostKeyChecking=no **/target/*.war ubuntu@${params.tomcat_prd}:~/apache-tomcat-8.5.28-prd/webapps"
                    }
                }
            }
        }
    }
}
