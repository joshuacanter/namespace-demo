# Tekton pipeline for creating namespace objects

. Apply 0-namespace.yaml to create the namespace
. Apply 1-sa.yaml
. Update tekton-triggers-eventlistener-clusterroles to allow access to ClusterInterceptor
. Apply 2-tasks.yaml
. Apply 3-pipeline.yaml
. Apply 4-triggers.yaml
. Create a route for the event listener
. Setup secret(referenced different ways by git and github tasks) #TODO: make one way to do this

`oc create secret generic my-basic-auth-secret --from-literal=.git-credentials=https://$USER:$TOKEN@github.com --from-literal=.gitconfig='[credential "https://github.com"]
  helper = store' --from-literal=token=$TOKEN`

## Testing
Example curl:
```
ROUTE=$(oc get route el-trigger-ref-listener -o jsonpath={.spec.host})

curl -k --location 'http://$ROUTE/' --header 'Event: external_request' --header 'Content-Type: application/json' --data '{
    "url": "https://github.com/joshuacanter/namespace-demo.git",
    "branch": "main",
    "namespace": "trigger-test-4"
}'
```