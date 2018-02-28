pipeline {
    agent any
    
    parameters {
        string(name: 'tomcat_dev', defaultValue: '35.176.168.64:8090', description: 'Staging Server')
        string(name: 'tomcat_prd', defaultValue: '35.176.168.64:9090', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving...'
                    archiveArtifats artifacts: '/home/ubuntu/warFile/*.war'
                }
            }
        }

        stage('Deployment'){
            parallel {
                stage('Deploy to Staging'){
                    steps {
                        sh 'scp -i /home/ubuntu/hieutr.pem /home/ubuntu/warFile/*.war ubuntu@${tomcat_dev}:~/apache-tomcat-8.5.28-stagg/webapps'
                    }
                }
                stage('Deploy to Production'){
                    steps {
                        sh 'scp -i /home/ubuntu/hieutr.pem /home/ubuntu/warFile/*.war ubuntu@${tomcat_prd}:~/apache-tomcat-8.5.28-stagg/webapps'
                    }
                }
            }
        }
    }
}
