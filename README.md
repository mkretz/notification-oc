# notification-oc
An OpenShift variant of the notification app

## How to deploy the backend components

### Prepare configuration
1. Create a sandbox account on [Mailgun](https://www.mailgun.com/)
1. Fill in the mailbox url and the to-address in `notification-mailer-configmap.yaml`

### Deploy the individual components
```
oc apply -f notification-db-volume-claim.yaml
oc apply -f notification-db-deployment.yml
oc apply -f notification-db-service.yml

oc apply -f notification-mq-deployment.yml
oc apply -f notification-mq-service.yaml

oc apply -f notification-api-deployment.yaml
oc apply -f notification-api-service.yml

oc apply -f notification-mailer-configmap.yaml
oc apply -f notification-mailer-deployment.yml
```

## How to deploy the UI components

### Option 1: from the Docker image
```
oc apply -f notification-ui-deployment.yaml
oc apply -f notification-ui-service.yaml
oc apply -f notification-ui-route.yaml
```

### Option 2: as an automated build triggered by a Git web hook

#### Prepare configuration
1. generate a secret (e.g. `openssl rand -hex 8`)
1. Fill in the secret in place of `<webhook-secret>` in `notification-ui-builder-bc.yaml` and `notification-ui-runtime-bc.yaml`

#### Deploy the image streams and builds
```
oc apply -f nginx-image-runtime.yaml
oc apply -f web-app-builder-runtime.yaml
oc apply -f notification-ui-runtime.yaml
oc apply -f notification-ui-builder.yaml
oc apply -f notification-ui-builder-bc.yaml
oc apply -f notification-ui-runtime-bc.yaml
oc apply -f notification-ui-deployment-config.yaml
oc apply -f notification-ui-service.yaml
oc apply -f notification-ui-route.yaml
```


## How to clean up
```
oc delete -f notification-mailer-deployment.yml
oc delete -f notification-mailer-configmap.yaml

oc delete -f notification-ui-route.yaml
oc delete -f notification-ui-service.yaml
oc delete -f notification-ui-deployment.yaml

oc delete -f nginx-image-runtime.yaml
oc delete -f web-app-builder-runtime.yaml
oc delete -f notification-ui-runtime.yaml
oc delete -f notification-ui-builder.yaml
oc delete -f notification-ui-builder-bc.yaml
oc delete -f notification-ui-runtime-bc.yaml
oc delete -f notification-ui-deployment-config.yaml
oc delete -f notification-ui-service.yaml

oc delete -f notification-api-service.yml
oc delete -f notification-api-deployment.yaml

oc delete -f notification-mq-service.yaml
oc delete -f notification-mq-deployment.yml

oc delete -f notification-db-service.yml
oc delete -f notification-db-deployment.yml
oc delete -f notification-db-volume-claim.yaml
```
