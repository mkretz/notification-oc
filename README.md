# notification-oc
An OpenShift variant of the notification app

## How to deploy

### Prepare configuration
1. Create a sanbox account on [Mailgun](https://www.mailgun.com/)
1. Fill in the mailbox url and the to-address in `notification-mailer-configmap.yaml`

### Deploy the individual components
```
oc apply -f notification-db-deployment.yml
oc apply -f notification-db-service.yml

oc apply -f notification-mq-deployment.yml
oc apply -f notification-mq-service.yaml

oc apply -f notification-api-deployment.yaml
oc apply -f notification-api-service.yml

oc apply -f notification-ui-deployment.yaml
oc apply -f notification-ui-service.yaml

oc apply -f notification-mailer-configmap.yaml
oc apply -f notification-mailer-deployment.yml
```

# How to clean up
```
oc delete -f notification-mailer-deployment.yml
oc delete -f notification-mailer-configmap.yaml

oc delete -f notification-ui-service.yaml
oc delete -f notification-ui-deployment.yaml

oc delete -f notification-api-service.yml
oc delete -f notification-api-deployment.yaml

oc delete -f notification-mq-service.yaml
oc delete -f notification-mq-deployment.yml

oc delete -f notification-db-service.yml
oc delete -f notification-db-deployment.yml
```
