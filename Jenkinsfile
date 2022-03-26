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
        git 'https://github.com/sportstube28/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
        container('centos') {
          stage('start calculator')
            {
              sh '''
                cd Chapter09/sample3
                #curl -ik -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"  https://kubernetes.default.svc.cluster.local/api/v1/namespaces/default/pods
                #curl -ik -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"  https://kubernetes.default.svc.cluster.local/apis/apps/v1/namespaces/default/deployments -X POST -H "Content-type: application/yaml" --data-binary @hazelcast.yaml
                #curl -ik -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"  https://kubernetes.default.svc.cluster.local/apis/apps/v1/namespaces/default/deployments -X POST -H "Content-type: application/yaml" --data-binary @calculator.yaml
                #curl -ik -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"  https://kubernetes.default.svc.cluster.local/api/v1/namespaces/default/pods
                curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                chmod +x ./kubectl
                ./kubectl apply -f calculator.yaml
                ''' }
                stage('Tesing Using the Curl')
                {
                  sh 'test $(curl calculator-service:8080/sum?a=1\\&b=2) -eq 3'
                  sh 'test $(curl calculator-service:8080/div?a=1\\&b=1)'
                }
          }
      }
    }
  }
