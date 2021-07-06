pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/srilekhasoma/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("srilekhasoma/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'DockerNew', url: 'https://registry.hub.docker.com') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy configs: '', credentialsType: 'Text', kubeConfig: [path: '/root/.minikube/ca.crt'], kubeconfigId: '', secretName: '', ssh: [sshCredentialsId: 'sonar', sshServer: 'https://192.168.49.2:8443'], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://192.168.49.2:8443']
        }
      }
    }

  }
}   
