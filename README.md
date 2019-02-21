```
jobs:

  - name: knative application
    plan:
      - params:
          kubectl: get nodes
          app_name: knative-sandboox
          repo:
            url: https://github.com/metacoma/knative-sandboox.git
            branch: master
          dockerhub:
            username: ((dockerhub_username))
            password: ((dockerhub_password))
        put: knative

resource_types:
  - name: knative
    source:
      #repository: aptomisvc/concourse-knative-resource
      repository: metacoma/concourse-resource-knative
      tag: gcloud4
    type: docker-image

resources:
  - name: knative
    source:
      kubeconfig: ((knative_kubeconfig))
      gke:
        service_account_json: ((service_account_json))
        account: "k8s-admin@aptomi-main-216807.iam.gserviceaccount.com"
        project: aptomi-main-216807
        zone: us-central1-a
        name: standard-cluster-1
    type: knative

```
```
jobs:
  - name: knative serving application
    plan:
      - params:
          kubectl: cluster-info
          app_name: rest-api
          image: metacoma/knative-serving
          namespace: default
        put: knative-serving

resource_types:
  - name: knative-serving
    source:
      #repository: aptomisvc/concourse-knative-resource
      repository: metacoma/concourse-knative-serving-resource
      tag: latest
    type: docker-image

resources:
  - name: knative-serving
    source:
      kubeconfig: ((knative_kubeconfig))
      gke:
        service_account_json: ((service_account_json))
        account: "k8s-admin@cool-project-main-206808.iam.gserviceaccount.com"
        project: some-cool-project
        zone: us-central1-b
        name: standard-cluster-1
    type: knative-serving
```
