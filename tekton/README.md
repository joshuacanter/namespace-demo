# Tekton pipeline for creating namespace objects

. Create 'namespace-creator' namespace
. Apply sa.yaml
. Update tekton-triggers-eventlistener-clusterroles to allow access to ClusterInterceptor
. Apply tasks.yaml
. Apply pipeline.yaml
. Create a route for the event listener
. Setup secret(referenced different ways by git and github tasks) #TODO: make one way to do this

`oc create secret generic my-basic-auth-secret --from-literal=.git-credentials=https://$USER:$TOKEN@github.com --from-literal=.gitconfig='[credential "https://github.com"]
  helper = store' --from-literal=token=$TOKEN`

## Testing
Example curl:

```
curl -k --location 'https://listener-namespace-creator.apps-crc.testing/' --header 'Event: external_request' --header 'Content-Type: application/json' --data '{
    "url": "https://github.com/joshuacanter/namespace-demo.git",
    "branch": "main",
    "namespace": "trigger-test"
}'
```