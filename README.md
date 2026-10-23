
*********minikube implementation******************

==================== 2 container creation  ================================================================

  Container1=ML  and  Container2=UI == separate cont. and 2 versions for UI only, roll updates and roll backs

ClusterIP service for ML and load balancer for UI

annotator is option
======================================================= Container. for ML-model

change/replace  ClusterIP with == replace with loadbalancer in service.yaml  for ML Cont.=============

kubectl create -f deployment.yaml

kubectl create -f service.yaml

======================================================= container. for UI web app

kubectl create -f placement-flask-deployment.yaml

kubectl create -f placement-flask-service.yaml

kubectl get all

=====================================================================================================

PS C:\Users\abcde\OneDrive\Desktop\MLOPS-ASSIGNMENTS\placement-webapp\minikube-ml-flask-web-app> kubectl get all                           
NAME                             READY   STATUS    RESTARTS   AGE
pod/flask-app-57d8dc8568-lzfnq   1/1     Running   0          4m2s
pod/flask-app-57d8dc8568-tsfmz   1/1     Running   0          3m40s
pod/placement-7457c85b7f-2gz9n   1/1     Running   0          16m
pod/placement-7457c85b7f-rj86q   1/1     Running   0          16m

NAME                        TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/flask-app-service   LoadBalancer   10.103.66.64    <pending>     5000:30226/TCP   15m
service/kubernetes          ClusterIP      10.96.0.1       <none>        443/TCP          18h
service/placement           LoadBalancer   10.106.122.17   <pending>     80:31605/TCP     16m

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/flask-app   2/2     2            2           15m
deployment.apps/placement   2/2     2            2           16m

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/flask-app-57d8dc8568   2         2         2       4m2s
replicaset.apps/flask-app-d777d79      0         0         0       15m
replicaset.apps/placement-7457c85b7f   2         2         2       16m
PS C:\Users\abcde\OneDrive\Desktop\MLOPS-ASSIGNMENTS\placement-webapp\minikube-ml-flask-web-app> 

===========================================================================================================

minikube service flask-app-service

http://127.0.0.1:57428 

 browser opens automatically , give port number as 80

why are we using port 80 = pod port number = user interact with pod only


same file deployment.yaml , only few changes done , so apply instead of create

kubectl apply -f placement-flask-deployment.yaml ===== apply this command at web-app v2 folder

== change from v1 to v2(infra changes)


=========response is configured=====================

minikube service flask-app-service

refresh UI

kubectl get deployments

Check the Docker image version

kubectl get deployments -o wide

3. Check the image used by each running Pod

kubectl get pods -o wide

kubectl describe pod flask-app


kubectl logs deployment/flask-app --tail=50

kubectl rollout history deployment flask-app  ==== give history of versions of service = v1, v2 etc.


kubectl rollout undo deployment flask-app --to-revision=1  ;   == undo v2 go to vesion- v1

kubectl rollout status deployment flask-app


kubectl set image deployment/placement placement=pandumlops/placement:new-v2  == deploy from v1 to v2

kubectl rollout status deployment/placement
kubectl get pods

=================================================================

deployment/placement — your Deployment name.


placement after that — your container name.


myusername/placement:v2 — your Docker Hub image with the v2 tag.

=============================================================================


kubectl get all

===============================================


kubectl logs <pod-name>    == static logs and exit terminal

kubectl logs pod/flask-app-66769888ff-8qrz2

kubectl logs pod/recruitment-rank-app-c5c76b4b4-k77v6

kubectl exec -it <pod-name> -- bash       ==  to access file system in pod/cont.

kubectl exec -it pod/recruitment-rank-app-c5c76b4b4-k77v6 -- bash

ls

exit  == exit from pod/cont.


kubectl cluster-info

kubectl delete deployments placement

kubectl delete service  placement

kubectl delete horizontalpodautoscaler  placement
