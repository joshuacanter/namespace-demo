# bootstrap

. Apply 0-operators.yaml
. Apply 1-instances.yaml
. Apply /tekton (follow README there)
. Give the sa for application controller more permissions (oc adm policy add-cluster-role-to-user cluster-admin -z openshift-gitops-argocd-application-controller DO NOT DO THIS ANYWHERE IMPORTANT, GIVE THE CONTROLLER CORRECT PERMISSIONS IN A ROLE)
. apply /applications/dev/application.yaml to openshift-gitops