# README

This is a Keptn Service Template written in GoLang. 

Quick start:

1. Click [Use this template](https://github.com/keptn-sandbox/freight-keptn-go/generate) on top of the repository, or download the repo as a zip-file, extract it into a new folder named after the service you want to create (e.g., simple-service) 
1. Replace every occurrence of (docker) image names and tags from `freightclear/freight-keptn-go` to your docker organization and image name (e.g., `yourorganization/simple-service`)
1. Replace every occurrence of `freight-keptn-go` with the name of your service (e.g., `simple-service`)
1. Optional (but recommended): Create a git repo (e.g., on `github.com/your-username/simple-service`)
1. Àdapt the [go.mod](go.mod) file and change `example.com/` to the actual package name (e.g., `github.com/your-username/simple-service`)
1. Add yourself to the [CODEOWNERS](CODEOWNERS) file
1. Initialize a git repository: 
  * `git init .`
  * `git add .`
  * `git commit -m "Initial Commit"`
1. Optional: Push your code an upstream git repo (e.g., GitHub) and adapt all links that contain `github.com` (e.g., to `github.com/your-username/simple-service`)
1. Last but not least: Remove this intro within the README file and make sure the README file properly states what this repository is about

# freight-keptn-go
![GitHub release (latest by date)](https://img.shields.io/github/v/release/keptn-sandbox/freight-keptn-go)
[![Go Report Card](https://goreportcard.com/badge/github.com/keptn-sandbox/freight-keptn-go)](https://goreportcard.com/report/github.com/keptn-sandbox/freight-keptn-go)

This implements a freight-keptn-go for Keptn. If you want to learn more about Keptn visit us on [keptn.sh](https://keptn.sh)

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

This should install the `freight-keptn-go` together with a Keptn `distributor` into the `keptn` namespace, which you can verify using

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

Development can be conducted using any GoLang compatible IDE/editor (e.g., Jetbrains GoLand, VSCode with Go plugins).

It is recommended to make use of branches as follows:

* `master` contains the latest potentially unstable version
* `release-*` contains a stable version of the service (e.g., `release-0.1.0` contains version 0.1.0)
* create a new branch for any changes that you are working on, e.g., `feature/my-cool-stuff` or `bug/overflow`
* once ready, create a pull request from that branch back to the `master` branch

When writing code, it is recommended to follow the coding style suggested by the [Golang community](https://github.com/golang/go/wiki/CodeReviewComments).

### Where to start

If you don't care about the details, your first entrypoint is [eventhandlers.go](eventhandlers.go). Within this file 
 you can add implementation for pre-defined Keptn Cloud events.
 
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

This repo uses [Travis-CI](https://travis-ci.org) to automatically build docker images. This process is optional and needs to be manually 
enabled by signing in into [travis-ci.org](https://travis-ci.org) using GitHub and enabling Travis for your repository.

After enabling Travis-CI, the following settings need to be added as secrets to your repository on the Travis-CI Repository Settings page:

* `REGISTRY_USER` - your DockerHub username
* `REGISTRY_PASSWORD` - a DockerHub [access token](https://hub.docker.com/settings/security) (alternatively, your DockerHub password)

Furthermore, the variable `IMAGE` needs to be configured properly in the respective section:
```yaml
env:
  global:
    - IMAGE=freightclear/freight-keptn-go # PLEASE CHANGE THE IMAGE NAME!!!
```
You can find the implementation of the build-job in [.travis.yml](.travis.yml).

## How to release a new version of this service

It is assumed that the current development takes place in the master branch (either via Pull Requests or directly).

To make use of the built-in automation using Travis CI for releasing a new version of this service, you should

* branch away from master to a branch called `release-x.y.z` (where `x.y.z` is your version),
* write release notes in the [releasenotes/](releasenotes/) folder,
* check the output of Travis CI builds for the release branch, 
* verify that your image was built and pushed to DockerHub with the right tags,
* update the image tags in [deploy/service.yaml], and
* test your service against a working Keptn installation.

If any problems occur, fix them in the release branch and test them again.

Once you have confirmed that everything works and your version is ready to go, you should

* create a new release on the release branch using the [GitHub releases page](https://github.com/keptn-sandbox/freight-keptn-go/releases), and
* merge any changes from the release branch back to the master branch.

## License

Please find more information in the [LICENSE](LICENSE) file.