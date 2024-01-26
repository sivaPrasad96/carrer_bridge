pipeline{
    agent any

    environment{
        PATH= "/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin:$PATH"
    }

    stages{
        stage("Git clone form Github"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/aakumar07/devops2aws_12-30-PM.git'
            }
        }

        stage("maven build & sonar scan"){
            steps{
                sh 'mvn -f /var/lib/jenkins/workspace/pipeline/creditcard/pom.xml clean sonar:sonar package'
            }
        }

        stage("articat Uploder using Nexus"){
            steps{
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'creditcard', 
                        classifier: '', 
                        file: '/var/lib/jenkins/workspace/pipeline/creditcard/target/creditcard.war', 
                        type: 'war']
                    ], 
                    credentialsId: 'nexus3', 
                    groupId: 'ICICI', 
                    nexusUrl: '172.31.26.246:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'sample-snapshot', 
                    version: '1.0-SNAPSHOT'
            }
        }

        stage("deploy to wat in Tomcat"){
            steps{
                sshagent(['tomcat']) {
                    sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline/creditcard/target/creditcard.war  ec2-user@3.141.244.75:/home/ec2-user/tomcat-9/webapps/'
                }
            }
        }
        }
    }
