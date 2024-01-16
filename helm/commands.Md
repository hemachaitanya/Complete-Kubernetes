# _____helm____

```sh
* helm create hellow-world - To create a hellow-world Helm Chart
* tree Helm Chart name - To verify the complete directory structure of the HelmChart
* helm install <Release Name>  <Actual Chart Name> - To remove the charts
  * helm install myhelloworld hello-world
* helm list -a -To show list of Charts
* helm uninstall <Release Name> - To uninstall  charts
* helm upgrade <Name of the Release Chart> <Actual Chart Name>
* helm rollback <Name of the Release Chart> <Release Number>
* helm install <Name of the Release Chart> --debug --dry-run <Actual Chart Name> - validate your helm cahrts before install
* helm template <Actual Chart Name> - renders the chart templates locally
* helm lint <Actual Chart Name> - Find any errors or misconfiguration
* helmfile synk or helm delete <Name of the Release Chart>
```
  
```yaml
* Fetch release history - helm history myrelease
* Search for a keyword in charts - helm search repo <keyword>
* Show information about a chart - helm show chart ./mychart
* Display the status of a named release - helm status myrelease
* Run tests for a release - helm test myrelease
* Package a chart directory into a chart archive - helm package ./mychart
* Add a chart repository -helm repo add stable https://charts.helm.sh/stable
* Install a Helm plugin - helm plugin install https://example.com/helm-plugin
* Upgrade a release to a specific version -helm upgrade myrelease ./mychart --version 1.2.3
* Login to a registry -helm registry login registry.example.com
* Push a chart to a remote repository -helm push ./mychart/registry.example.com/myrepo
```
