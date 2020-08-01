#It's hard for us to use the deployment provided by official document because of the images problem.

#So I used the deployment in this file to deploy tekton and trigger by the order in the following.

kubectl apply -f release.yaml

kubectl apply -f tekton-trigger-release.yaml
