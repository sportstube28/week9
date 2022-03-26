podTemplate(yaml: '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: centos
    image: centos
    command:
    - sleep
    args:
    - 99d
  restartPolicy: Never
''') {
    node(POD_LABEL) {
      stage('k8s')
      {
        git url: 'https://github.com/sportstube28/week9.git', branch: 'main'
        container('centos') {
          stage('Deploy')
            {
              sh '''
                curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                chmod +x ./kubectl
                ./kubectl apply -f calculator.yaml -n staging
                ./kubectl apply -f hazelcast.yaml -n staging
                '''
              }
          stage('Tesing Using the Curl')
            {
            sh 'test $(curl calculator-service.staging.svc.cluster.local:8080/sum?a=1\\&b=2) -eq 3'
            sh 'test $(curl calculator-service.staging.svc.cluster.local:8080/div?a=1\\&b=1)'
          }
        }
      }
    }
  }
