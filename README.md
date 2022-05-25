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
Run Jenkins in docker:
docker run --name jenkins-docker --rm --detach --privileged \
--network jenkins --network-alias docker \
--env DOCKER_TLS_CERTDIR=/certs \
--volume jenkins-docker-certs:/certs/client \
--volume jenkins-data:/var/jenkins_home \
--publish 2376:2376 docker:dind \
--storage-driver overlay2
```
```
docker run --name jenkins-blueocean2 \
--restart=on-failure --detach --network jenkins \
--env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
--publish 8080:8080 --publish 50000:50000 --publish 9100:9100 \
--volume jenkins-data:/var/jenkins_home \
--volume jenkins-docker-certs:/certs/client:ro \
myjenkins-blueocean:2.332.3-1
```

ttt