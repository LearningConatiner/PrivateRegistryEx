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

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ vi secret64
The value of the .dockercfg field is a base64 representation of your secret data.
Copy the base64 representation of the secret data into a file named secret64.
Important: Make sure there are no line breaks in your secret64 file.
To understand what is in the .dockercfg field, convert the secret data to a readable format:

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ base64 -d secret64

The output is similar to this:
{"yourprivateregistry.com":{"username":"janedoe","password":"xxxxxxxxxxx","email":"jdoe@example.com","auth":"c3R...zE2"}}

5.Create a docker image and push to private registry
docker build -t privatehelloworld .
Sending build context to Docker daemon 69.63 kB
Step 1 : FROM python:3.5-alpine
3.5-alpine: Pulling from library/python
486a8e636d62: Pull complete 
6e3b49f9e4bd: Pull complete 

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
privatehelloworld   latest              cfe6742a4988        7 seconds ago       99.59 MB
python              3.5-alpine          401ea9b24eeb        5 days ago          89.32 MB

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker push srilakh/privateregistrydemo
The push refers to a repository [docker.io/srilakh/privateregistrydemo]
a9b5564eed6f: Pushed 
f2cba49d28b2: Pushed 
d5a792b4f4ce: Pushed 
668afe561231: Pushed 
f2260405fef6: Mounted from library/python 
9b0e788ce427: Mounted from library/python 
924d96b45493: Mounted from library/python 
edaed5399789: Mounted from library/python 
8539d1fe4fab: Mounted from library/python 
privatehello: digest: sha256:3e4714464af62352c4c11c5807793ed021998b54aba4eaaf084bb20f3d344cad size: 2200

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker rmi -f cfe6742a4988
Untagged: privatehelloworld:latest
Untagged: srilakh/privateregistrydemo:privatehello
Deleted: sha256:cfe6742a49887ee22021a19b82056800054ee408f446cac777c6e7b4c0b12b7e
Deleted: sha256:c544da74d08f6f8a228cc235c275377ed2c3d4f0c16a0f8198036c3fab983b8a
Deleted: sha256:a4964372bb0ec654a6a3704adab00543088f803559161d593eb88751140e72bc
Deleted: sha256:15d4331527f0f3cb07d91408a4b16aff02118ccf19106f6ce8e62a5188a22e29
Deleted: sha256:a1691d64286b9c3166b61a0de6635286f7704619f80fd0816a85b35c074cfe2f
Deleted: sha256:3a5330f074eb775e695712ce218c6569a19d2869c61e0a2edf6f1f04021c0af4
Deleted: sha256:3cebddccd6199efee6a742f7de1ac4ac0f16219aa7bb675a2f34695773e8cec4
Deleted: sha256:6267ebf17b1e86499fc2f41e584eb7deceb8d5b953916b3c7d448220fbd7fe35
Deleted: sha256:dbf12e33bfc36bd49fa03d27cd16c6674cf9930514e6ff263c2cafcc46ec6d99
Deleted: sha256:17171d6ad1402cdddbb99c71fe5380b56460d9a86afe9d8de90ec835a427fc23
Deleted: sha256:29d0dc43293adbb6b63db7aac09a4a4e49b97dc76a84c7e1eeda3f197b71f375
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python              3.5-alpine          401ea9b24eeb        5 days ago          89.32 MB
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker pull srilakh/privateregistrydemo:privatehello
privatehello: Pulling from srilakh/privateregistrydemo
486a8e636d62: Already exists 
6e3b49f9e4bd: Already exists 
b09b1e5f1956: Already exists 
d9560d8574eb: Already exists 
df50ca023b09: Already exists 
861a76790d00: Pull complete 
215908961bed: Pull complete 
76958c92732a: Pull complete 
b95b6bba7753: Pull complete 
Digest: sha256:3e4714464af62352c4c11c5807793ed021998b54aba4eaaf084bb20f3d344cad
Status: Downloaded newer image for srilakh/privateregistrydemo:privatehello
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
srilakh/privateregistrydemo   privatehello        cfe6742a4988        3 minutes ago       99.59 MB
python                        3.5-alpine          401ea9b24eeb        5 days ago          89.32 MB
b09b1e5f1956: Pull complete 

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
privatehelloworld   latest              cfe6742a4988        7 seconds ago       99.59 MB
python              3.5-alpine          401ea9b24eeb        5 days ago          89.32 MB

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker push srilakh/privateregistrydemo
The push refers to a repository [docker.io/srilakh/privateregistrydemo]
a9b5564eed6f: Pushed 
f2cba49d28b2: Pushed 
d5a792b4f4ce: Pushed 
668afe561231: Pushed 
f2260405fef6: Mounted from library/python 
9b0e788ce427: Mounted from library/python 
924d96b45493: Mounted from library/python 
edaed5399789: Mounted from library/python 
8539d1fe4fab: Mounted from library/python 
privatehello: digest: sha256:3e4714464af62352c4c11c5807793ed021998b54aba4eaaf084bb20f3d344cad size: 2200

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker rmi -f cfe6742a4988
Untagged: privatehelloworld:latest
Untagged: srilakh/privateregistrydemo:privatehello
Deleted: sha256:cfe6742a49887ee22021a19b82056800054ee408f446cac777c6e7b4c0b12b7e
Deleted: sha256:c544da74d08f6f8a228cc235c275377ed2c3d4f0c16a0f8198036c3fab983b8a
Deleted: sha256:a4964372bb0ec654a6a3704adab00543088f803559161d593eb88751140e72bc
Deleted: sha256:15d4331527f0f3cb07d91408a4b16aff02118ccf19106f6ce8e62a5188a22e29
Deleted: sha256:a1691d64286b9c3166b61a0de6635286f7704619f80fd0816a85b35c074cfe2f
Deleted: sha256:3a5330f074eb775e695712ce218c6569a19d2869c61e0a2edf6f1f04021c0af4
Deleted: sha256:3cebddccd6199efee6a742f7de1ac4ac0f16219aa7bb675a2f34695773e8cec4
Deleted: sha256:6267ebf17b1e86499fc2f41e584eb7deceb8d5b953916b3c7d448220fbd7fe35
Deleted: sha256:dbf12e33bfc36bd49fa03d27cd16c6674cf9930514e6ff263c2cafcc46ec6d99
Deleted: sha256:17171d6ad1402cdddbb99c71fe5380b56460d9a86afe9d8de90ec835a427fc23
Deleted: sha256:29d0dc43293adbb6b63db7aac09a4a4e49b97dc76a84c7e1eeda3f197b71f375
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python              3.5-alpine          401ea9b24eeb        5 days ago          89.32 MB
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker pull srilakh/privateregistrydemo:privatehello
privatehello: Pulling from srilakh/privateregistrydemo
486a8e636d62: Already exists 
6e3b49f9e4bd: Already exists 
b09b1e5f1956: Already exists 
d9560d8574eb: Already exists 
df50ca023b09: Already exists 
861a76790d00: Pull complete 
215908961bed: Pull complete 
76958c92732a: Pull complete 
b95b6bba7753: Pull complete 
Digest: sha256:3e4714464af62352c4c11c5807793ed021998b54aba4eaaf084bb20f3d344cad
Status: Downloaded newer image for srilakh/privateregistrydemo:privatehello
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
srilakh/privateregistrydemo   privatehello        cfe6742a4988        3 minutes ago       99.59 MB
python                        3.5-alpine          401ea9b24eeb        5 days ago          89.32 MB


Create a pod with the yaml file 
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/Step1$ kubectl create -f private-reg-pod.yaml 
pod "private-reg" created

kubectl get pod private-reg
