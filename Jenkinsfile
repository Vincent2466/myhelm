// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            // Or, to avoid YAML:
            // containerTemplate {
            //     name 'shell'
            //     image 'ubuntu'
            //     command 'sleep'
            //     args 'infinity'
            // }
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: anandsadhu/dotnet-jenkins-slave
    command:
    - sleep
    args:
    - infinity
    tty: true
    resources:
      requests:
        memory: "2Gi"
    volumeMounts:
     - name: docker-sock
       mountPath: /var/run/docker.sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock       
'''
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            defaultContainer 'shell'
        }
    }
   stages {
        stage('docker build') {
            steps {
                sh 'docker build -t vincent53/demoapp .'
       }
    }
   stage('docker login') {
       steps {
            sh 'docker login -u vincent563 -p vincentbabu'
        }
    }
    stage('docker push') {
        steps {
            sh 'docker push vincent53/demoapp'
           }
    }
     stage("install helm"){
       steps {
         sh 'curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3'
         sh 'chmod 700 get_helm.sh'
         sh './get_helm.sh'
           }
        }
       stage('helm') {
           steps {
               sh 'helm version'
               sh 'helm upgrade --install testapp9 test-app'
 
             
           }
       }
     }
  }
