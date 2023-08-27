# Assignment Question:

- Create a new repo for managing argo files.
- Create an argocd application architecture to deploy k8s manifest files for two clusters. (use application, applicationsets, projects) (at-least 8-10 sample apps should be deployed)
- Deploy a kustomize application to 2 different cluster as prod and stage with one base.
- All the sync should be automated.
- Add git webhook which can trigger the sync as soon as any changes are pushed to repo.
- Should get flock msg for each step of sync Pre, post etc.


# How it works:
1. main application is appset.yaml in the base of the repository which deploys 2 applications - prod and dev.
2. Prod deploys the appset/prod kustomization and dev deploys appset/dev kustomization, which contains kustomizations and some resources in base.
3. Both prod and dev have base in appset/base which deploys 3 kind of apps:
    * both prod and dev are deployed on separate namespaces named prod and dev respectively.
    * Single Application - nginx
    * Appset with resource in same repo - pulled some helm charts using command like `helm pull bitnami/mysql --untar` into the directory. 
    * Reduced the number of deployments as it was not deploying properly and slowed the cluster due to resource constraints.
    * Appset with resources from remote repo - some repos containing heml charts/k8s deployments
4. prod is deployed on Ashwin's cluster and dev on Shubham's cluster. The repo is deployed on shubham's Argo deployment. 
5. Argo is deployed using argo helm charts and creating a Loadbalancer to expose the argo endpoint.
6. After that logging in into other person's argo using argocd cli using the following commands.
```bash
argocd login <ip>:port` 
# and then running 
argocd cluster join <contextname>
```
7. Both argo clusters were deployed on GCP. 
8. Github hooks are added to automatically sync on pushes to the repository, and argo hooks are added to send flock notifications on presync and postsync. Github hooks are added via Github.com -> settings -> webhooks.

9. The repository contains contribution from both the team members and both worked on it together.

# Resource Links:
- Screenshots: https://drive.google.com/drive/folders/1x0QRnzonN2FHikaK8HLmpwhPNKO8yZAvoo?usp=sharing
- Githook working video: https://drive.google.com/file/d/1BlqVFrnltoZTeInKGfnbG898bS3qthig/view?usp=drive_link
