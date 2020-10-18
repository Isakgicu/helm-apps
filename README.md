## Create helm app

```
$ helm create <chart-name>
$ helm package <chart-name>
$ mv <chart-name>-0.1.0.tgz docs
$ helm repo index --url https://isakgicu.github.io/helm-apps/
$ git add -i
$ git commit -av
$ git push origin master
```

## Add repository to your helm library
```bash
helm repo add isak-apps https://isakgicu.github.io/helm-apps/
```

Then You can run:
```bash
helm install isak-apps/<chart-name>
```


To release a new version of an application, for example `generic-app`:
* Make your changes and then edit `charts/generic-app/Chart.yaml`, update version number
* `cd charts`
* `helm package generic-app`
* `mv generic-app-X.Y.Z.tgz ../`
* `cd ..`
* `helm repo index --url https://isakgicu.github.io/helm-apps/`
* Commit and push to the repo
* Tag the release `git tag vX.Y.X`
* Push tags `git push --tags`
* Update your local repo `helm repo update`