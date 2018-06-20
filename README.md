# Microservices: FoodPal

A microservice based Node.js application for sharing food.

Components:

* Auth service
* Feed service
* Notification service

## Building

1. Build each microservice as a docker image.

    ```bash
    docker build -t botix/foodpal-auth fooodpal-auth/
    docker build -t botix/foodpal-feed-service fooodpal-feed-service/
    docker build -t botix/foodpal-notification-service fooodpal-notification-service/
    ```

2. Upload the latest, modified images to Docker Hub.

    ```bash
    docker push botix/foodpal-auth
    docker push botix/foodpal-feed-service
    docker push botix/foodpal-notification-service
    ```

## Deploying

To make deployment easier, each microservice has a corresponding `.yaml` file, describing a service and distribution.

1. Deploy secret used for JWT hashing.

    ```bash
    kubectl create -f password_hash.yaml
    ```

2. Deploy services.

    ```bash
    # Deploy a MongoDB instance
    kubectl create -f mongo.yaml
    # Deploy auth-service
    kubectl create -f foodpal-auth.yaml
    # Deploy feed-service
    kubectl create -f foodpal-feed-service.yaml
    # Deploy notification-service
    kubectl create -f foodpal-notification-service.yaml
    ```

3. Optionally create Ingress proxy to give external access to foodpal services.

    ```bash
    kubectl create -f foodpal-ingress.yaml
    ```

## Minikue

If you're running Kubernetes in Minikube, make sure you enable the Ingress addon before creating with kubectl.

    ```bash
    minikube addons enable ingress
    ```