Steps to package the application using Docker commands:

``` 
# build the image
docker build -t go-helloworld .

# run the image
docker run -d -p 6112:6112 go-helloworld

# tag the image
docker tag go-helloworld nduati/go-helloworld:v1.0.0

# push the image
docker push nduati/go-helloworld:v1.0.0

# login into DockerHub
docker login
```

Deploy to Kubernetes:
```
# run the application
kubectl run test --image nduati/go-helloworld:v1.0.0

# access the application on the local host
kubectl port-forward test-97856cf4-6fvjw 6112:6112
```
