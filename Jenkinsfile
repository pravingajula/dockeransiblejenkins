pipeline{
    agent any
    environment {
    Docker_Tag = getVersion()
    }
    
    tools{
        maven 'maven3.6.1'
    }
    
    stages{
        stage('Scm Checkout'){
            steps{
                script{
                    git url: 'https://github.com/pravingajula/dockeransiblejenkins.git', branch: 'master'
                }
                   
            }    
            
        }
        stage('mvn'){
            steps{
                script{
                
                //def MVN = tool name: 'maven3.6.1', type: 'maven'
                //def MVNCMD = "${MVN}/bin/mvn"
                //sh "${MVNCMD} clean package"
                         //or we can give this
                         
                 sh 'mvn clean package' 
                
                }
            }
        }
        stage('docker image'){
            steps{
                script{
                    sh "docker build . -t pravin786/withans:${Docker_Tag}"
                }
            }
        }
        stage('docker push'){
            steps{
                script{
                    //using with credentials
                    //withCredentials([string(credentialsId: 'docker_hub_pwd', variable: 'DockerPwd')]) {
                    //sh "docker login -u pravin786 -p ${DockerPwd}"
                    sh "docker login -u pravin786 -p ${dockerpwd}"    
                //}
                    sh "docker push pravin786/withans:${Docker_Tag}"
                    sh "docker rmi -f pravin786/withans:${Docker_Tag}"
                }
            }
        }
        stage('docker run using ansuble '){
            steps{
                script{
                    ansiblePlaybook credentialsId: 'ansible_server', disableHostKeyChecking: true, extras:  "-e Docker_tag=${Docker_Tag}", installation: 'Ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
                }
            }
        }
    }
}
def getVersion(){
    //got to pipeline syntax click Snippet Generator find sh: shell script to generate code 
    def Commitid = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return Commitid
    //to call this add globaly
}
