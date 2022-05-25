# Jenkins CI\CD\CD
Jenkins plugins: _(not mandatory for this example)_
- Kubernetes Continuous Deploy 1.0.0 (anyway, issue with ingress deployment exists)
- Ansible plugin 1.1

Notes:
```
docker {
         image 'bee7ch/ansible'                   // image with ansible and python 
         args '-it -u 0:0 --entrypoint='          // -u 0:0 workaround about 1000:1000 user missing in /etc/passwd
       }                                          // '--entrypoint=' override entrypoint to empty, because jenkins has own entrypoint
```
kubeconfig in env for ansible to connect:
```
           steps {
               withCredentials([file(credentialsId: 'kube-config-microk8s', variable: 'KUBECONFIG')]) {
                    ...
                    ...
               }
           }
```
####test