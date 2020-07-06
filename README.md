# Keptn Autoscale Cluster

# freight-keptn-go
![GitHub release (latest by date)](https://img.shields.io/github/v/release/keptn-sandbox/freight-keptn-go)
[![Go Report Card](https://goreportcard.com/badge/github.com/keptn-sandbox/freight-keptn-go)](https://goreportcard.com/report/github.com/keptn-sandbox/freight-keptn-go)

This implements a freight-keptn-go for Keptn. If you want to learn more about Keptn visit them on [keptn.sh](https://keptn.sh)

## Compatibility Matrix

| Keptn Version    | [freight-keptn-go Docker Image](https://hub.docker.com/r/freightclear/freight-keptn-go/tags) |
|:----------------:|:----------------------------------------:|
|       0.6.1      | freightclear/freight-keptn-go:0.1.0 |

## Installation

The *freight-keptn-go* can be installed as a part of [Keptn's uniform](https://keptn.sh).

### Deploy in your Kubernetes cluster

To deploy the current version of the *freight-keptn-go* in your Keptn Kubernetes cluster, apply the [`deploy/service.yaml`](deploy/service.yaml) file:

```console
kubectl apply -f deploy/service.yaml
```

This should install the `freight-keptn-go` together with a Keptn `distributor` into the `freight-keptn` namespace, which you can verify using

```console
kubectl -n keptn get deployment freight-keptn-go -o wide
kubectl -n keptn get pods -l run=freight-keptn-go
```

### Up- or Downgrading

Adapt and use the following command in case you want to up- or downgrade your installed version (specified by the `$VERSION` placeholder):

```console
kubectl -n keptn set image deployment/freight-keptn-go freight-keptn-go=freightclear/freight-keptn-go:$VERSION --record
```

### Uninstall

To delete a deployed *freight-keptn-go*, use the file `deploy/*.yaml` files from this repository and delete the Kubernetes resources:

```console
kubectl delete -f deploy/service.yaml
```

## Development

It is recommended to make use of branches as follows:

* `master` contains the latest potentially unstable version
* `release-*` contains a stable version of the service (e.g., `release-0.1.0` contains version 0.1.0)
* create a new branch for any changes that you are working on, e.g., `feature/my-cool-stuff` or `bug/overflow`
* once ready, create a pull request from that branch back to the `master` branch

When writing code, it is recommended to follow the coding style suggested by the [Golang community](https://github.com/golang/go/wiki/CodeReviewComments).

### Where to start
 
To better understand Keptn CloudEvents, please look at the [Keptn Spec](https://github.com/keptn/spec).
 
If you want to get more insights, please look into [main.go](main.go), [deploy/service.yaml](deploy/service.yaml),
 consult the [Keptn docs](https://keptn.sh/docs/) as well as existing [Keptn Core](https://github.com/keptn/keptn) and
 [Keptn Contrib](https://github.com/keptn-contrib/) services.

### Common tasks

* Build the binary: `go build -ldflags '-linkmode=external' -v -o freight-keptn-go`
* Run tests: `go test -race -v ./...`
* Build the docker image: `docker build . -t freightclear/freight-keptn-go:dev` (Note: Ensure that you use the correct DockerHub account/organization)
* Run the docker image locally: `docker run --rm -it -p 8080:8080 freightclear/freight-keptn-go:dev`
* Push the docker image to DockerHub: `docker push freightclear/freight-keptn-go:dev` (Note: Ensure that you use the correct DockerHub account/organization)
* Deploy the service using `kubectl`: `kubectl apply -f deploy/`
* Delete/undeploy the service using `kubectl`: `kubectl delete -f deploy/`
* Watch the deployment using `kubectl`: `kubectl -n keptn get deployment freight-keptn-go -o wide`
* Get logs using `kubectl`: `kubectl -n keptn logs deployment/freight-keptn-go -f`
* Watch the deployed pods using `kubectl`: `kubectl -n keptn get pods -l run=freight-keptn-go`
* Deploy the service using [Skaffold](https://skaffold.dev/): `skaffold run --default-repo=your-docker-registry --tail` (Note: Replace `your-docker-registry` with your DockerHub username; also make sure to adapt the image name in [skaffold.yaml](skaffold.yaml))


### Testing Cloud Events

We have dummy cloud-events in the form of [RFC 2616](https://ietf.org/rfc/rfc2616.txt) requests in the [test-events/](test-events/) directory. These can be easily executed using third party plugins such as the [Huachao Mao REST Client in VS Code](https://marketplace.visualstudio.com/items?itemName=humao.rest-client).

## Automation

### GitHub Actions: Automated Pull Request Review

This repo uses [reviewdog](https://github.com/reviewdog/reviewdog) for automated reviews of Pull Requests. 

You can find the details in [.github/workflows/reviewdog.yml](.github/workflows/reviewdog.yml).

### GitHub Actions: Unit Tests

This repo has automated unit tests for pull requests. 

You can find the details in [.github/workflows/tests.yml](.github/workflows/tests.yml).

### Travis-CI: Build Docker Images

This repo uses [Travis-CI](https://travis-ci.org) to automatically build docker images. 
You can find the implementation of the build-job in [.travis.yml](.travis.yml).

## Releases

If any problems occur, fix them in the release branch and test them again.

Once you have confirmed that everything works and your version is ready to go, you should

* create a new release on the release branch using the [GitHub releases page](https://github.com/keptn-sandbox/freight-keptn-go/releases), and
* merge any changes from the release branch back to the master branch.

## License

Please find more information in the [LICENSE](LICENSE) file.
