# helm-charts

### How update chart template
- Update templates and add new values on ´values.yaml´ if necessary
- Test the chart with `helm test <chart-name>`

example: helm template generic-application ./charts/generic-application  -f ../flyfood-gitops/k8s/apps/flyfood-api/dev-values.yaml --debug 

- use ´helm lint ./charts/generic-application´
- use ´helm package ./charts/generic-application´
- commit and deploy