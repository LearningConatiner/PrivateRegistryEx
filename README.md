# PrivateRegistryEx
1.Login to docker using docker login
Use the docker hub credentials to login

srilakshmi_krishnamoorthy@x-jigsaw-168411:~$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.dock
er.com to create one.
Username: srilakh
Password: 
Login Succeeded
srilakshmi_krishnamoorthy@x-jigsaw-168411:~$ 

2.The login process creates or updates a config.json file that holds an authorization token.
View the config.json file:
cat ~/.docker/config.json
srilakshmi_krishnamoorthy@x-jigsaw-168411:~$ cat ~/.docker/config.json
{
        "auths": {
                "https://index.docker.io/v1/": {
                        "auth": "c3JpbGFraDpnb29nbGVAMTIz"
                }
        }
}srilakshmi_krishnamoorthy@x-jigsaw-168411:~$ 

3.Create a Secret named regsecret:

Create a Secret that holds your authorization token

Create a Secret named regsecret:
kubectl create secret docker-registry regsecret --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>

srilakshmi_krishnamoorthy@x-jigsaw-168411:~$ kubectl create secret docker-registry regsecret --docker-server=srilakh/privatere
istryDemo --docker-username=srilakh --docker-password=google@123 --docker-email=srilakshmi.krishnamoorthy@gmail.com
secret "regsecret" created

4.To understand what’s in the Secret you just created, start by viewing the Secret in YAML format:
kubectl get secret regsecret --output=yaml

srilakshmi_krishnamoorthy@x-jigsaw-168411:~$ kubectl get secret regsecret --output=yaml
apiVersion: v1
data:
  .dockercfg: eyJzcmlsYWtoL3ByaXZhdGVyZWdpc3RyeURlbW8iOnsidXNlcm5hbWUiOiJzcmlsYWtoIiwicGFzc3dvcmQiOiJnb29nbGVAMTIzIiwiZW1haWwiO
iJzcmlsYWtzaG1pLmtyaXNobmFtb29ydGh5QGdtYWlsLmNvbSIsImF1dGgiOiJjM0pwYkdGcmFEcG5iMjluYkdWQU1USXoifX0=
kind: Secret
metadata:
  creationTimestamp: 2017-06-14T15:27:43Z
  name: regsecret
  namespace: default
  resourceVersion: "41657"
  selfLink: /api/v1/namespaces/default/secrets/regsecret
  uid: 027cacdc-5116-11e7-ba09-42010a8000a1
type: kubernetes.io/dockercfg

5.

