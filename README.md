# ProjectedVolumeEx
1. Create the Secrets:
 # Create files containing the username and password:
 echo -n "admin" > ./username.txt
 echo -n "1f2d1e2e67df" > ./password.txt

 # Package these files into secrets:
 kubectl create secret generic user --from-file=./username.txt
 kubectl create secret generic pass --from-file=./password.txt

  srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$ vi projected-volume.yaml
  srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$ echo -n "admin" > ./username.txt
  srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$ echo -n "1f2d1e2e67df" > ./password.txt
  srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$ kubectl create secret generic user --from-file=./username.txt
secret "user" created
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$  kubectl create secret generic pass --from-file=./password.txt
secret "pass" created 

2.Create the Pod:
 kubectl create -f projected-volume.yaml
Verify that the Pod’s Container is running, and then watch for changes to the Pod:
 kubectl get --watch pod test-projected-volume
The output looks like this:

In another terminal, get a shell to the running Container:
 kubectl exec -it test-projected-volume -- /bin/sh
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$ kubectl create -f projected-volume.yaml
pod "test-projected-volume" created
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$  kubectl get --watch pod test-projected-volume
NAME                    READY     STATUS    RESTARTS   AGE
test-projected-volume   1/1       Running   0          7s

In your shell, verify that the projected-volumes directory contains your projected sources:
 / # ls projected-volumes/
 srilakshmi_krishnamoorthy@x-jigsaw-168411:~$ cd kubernetesex/projectedvolumeex/
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$ kubectl exec -it test-projected-volume -- /bin/sh
/ # ls 
bin               etc               proc              root              tmp               var
dev               home              projected-volume  sys               usr
/ # ls projected-volume/
password.txt  username.txt
/ # cat projected-volume/password.txt 
/ # cat projected-volume/username.txt 
admin/ # 
