#!/usr/bin/env groovy
def GIT_PATH = 'git@github.com:ZahiCahana/hello-world.git'
def GIT_CRED = 'jenkins_github_ssh_key'
def EMAIL_ADDR = 'zahi.cahana@imperva.com'



pipeline {
    agent {
        label 'il-dockerbuild-lp1'
    }
    
    tools {
        maven 'maven-3.3.9'    
    
    }
    options
    {
        buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5'))
        gitLabConnection('gitlab')
    }
   

    stages {
        stage('Git Clone') {
            steps {
               
                checkout([
                        $class                           : 'GitSCM',
                        branches                         : [[name: '*/master']],
                        doGenerateSubmoduleConfigurations: false,
                        extensions                       : [],
                        submoduleCfg                     : [],
                        userRemoteConfigs                : [[credentialsId: GIT_CRED, url: GIT_PATH]]
                ])
            }
        }
        stage('Build') {
            steps {

                sh "echo 'hi demo class'"
            }
        }
        
        stage('BlackDuck_Scan') {
            steps {

                synopsys_detect '--blackduck.trust.cert=true --detect.project.name=test_github --detect.project.version.name=1 --detect.cleanup=true --detect.source.path=${WORKSPACE}  --detect.excluded.detector.types=NUGET '
          
            }
        }
       
    }
    post {
        success {
            
            emailext(
                    to: EMAIL_ADDR,
                    subject: "Success Pipeline: ${currentBuild.fullDisplayName}",
                    body: "Pipeline URL: ${env.BUILD_URL}",
                    mimeType: 'text/html'
            )
        }

        failure {
           
            emailext(
                    to: EMAIL_ADDR,
                    subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                    body: "Pipeline URL: ${env.BUILD_URL}",
                    mimeType: 'text/html'
            )
        }
    }
} // pipeline
