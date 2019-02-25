```
jobs:
  - name: knative serving application
    plan:
      - get: serverless-image
        trigger: true
      - params:
          kubectl: cluster-info
          app_name: rest-api
          image: metacoma/knative-serving
          namespace: default
        put: knative-serving

resource_types:
  - name: knative-serving
    source:
      repository: aptomisvc/concourse-knative-serving-resource
      tag: latest
    type: docker-image

resources:
  - name: knative-serving
    source:
      kubeconfig: ((knative_kubeconfig))
    type: knative-serving
  - name: serverless-image
    source:
      repository: aptomisvc/rest-api-serverless
      tag: latest
```
