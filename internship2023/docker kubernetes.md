```
docker prune -a

minikube image load second-image

STEPS:
	1. sbt pack
	2. build image
	3. load image in minikube with: minikube image load <image_name>
	4. apply and test.
```
