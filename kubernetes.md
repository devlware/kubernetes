Some articles from the weave website with some interesting concepts explaining their use of prometheus and kubernetes.

https://www.weave.works/blog/prometheus-kubernetes-perfect-match/
https://www.weave.works/blog/prometheus-kubernetes-deploying/
https://www.weave.works/blog/prometheus-and-kubernetes-monitoring-your-applications/

To access the kubernetes UI on my localhost:

% kubectl proxy then
And then open the browser pointing at http://127.0.0.1:8001/ui

To get all the pods on all namespaces
kubectl get pods --all-namespaces

To get the name of the replication controller:
% kubectl get rc

If I had any replication controller I could use the command kubectl scale replicas=1 rc/<replication-controller-name>

This way I could change the number of replicas of a pod without using the pod file name.

Its also possible to change the number of replicas up and down using the yml file this way:
% kubectl scale --replicas=3 -f path-to-file-name-that-describes-replication-controller.yml

To delete the replication controller we can use:
% kubectl delete rc/<replication-name>


Another commands on deployment:

% kubectl get deploy

# information on replica set
% kubectl get rs 

% kubectl get pods --show-labels

% kubectl rollout status deployment/<name>

# roda uma imagem com a versão 2.
% kubectl set image deployment/<name> k8s-demo:=k8s-demo:2 

-----------------------------
To give a node a Label:

kubectl label nodes node1 hardware=high-spec
kubectl label nodes node2 hardware=low-spec

inside the yml file then we use the commands inside the spec session.
nodeSelector:
  hardware: high-spec

To get the labels of the currently nodes
% kubectl get nodes --show-labels

Its good to use labels to filter where a pod can run or not. For example, if we define that we have a node with labels env:qa or env:prod and we have inside the deployment file the nodeSelector with env:test the this pod will NOT run because there is no node that have the env:test defined. This way we can filter and avoid that certain pods run on the wrong node.

-----------------------------
To run a life check in the pod we can put the following commands in the spec.

livenessProbe:
  failureThreshold: 3
  httpGet:
    path: /
    port: nodes-port (port define for the service and accessible by any pod)
    scheme: HTTP
  initialDelaySeconds: 15
  periodSeconds: 10
  successThreshold: 1
  timeoutSeconds: 30

this way our kubesystem will check the health of our pod for us. This is why Gerson and Andre would like to have a simple http server inside the application to get the health of the system.
  
-----------------------------
SECRETS

one way to create secrets file is:

echo -n "root" > ./username.txt
echo -n "password" > ./password.txt
kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt

SSH ou SSL secrets:

kubectl create secret generic ss-certificate --from-file=ssh-privatekey=~/.sshid_rsa --ssl-cert=ssl-cert=mysslcert.crt

Another option is:


Setting the namespace preference
You can permanently save the namespace for all subsequent kubectl commands in that context.
$ kubectl config set-context $(kubectl config current-context) --namespace=<insert-namespace-name-here>
# Validate it
$ kubectl config view | grep namespace:


# delete all pods
kubectl delete --all pods --namespace=crazy-name-space

# delete all deployments
kubectl delete --all deployments --namespace=crazy-name-space

# delete all services
kubectl delete --all services --namespace=crazy-name-space


# Command to run an image X on port Y in namespace monitoring
kubectl run prometheus-deploy --image=gcr.io/my-project-name/prometheus:master --port=9090 -n monitoring
