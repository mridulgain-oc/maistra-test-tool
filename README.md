# Maistra OpenShift Test Tool

[![](https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=flat)](https://github.com/maistra/maistra-test-tool/blob/development/LICENSE)
![](https://img.shields.io/github/repo-size/maistra/maistra-test-tool.svg?style=flat)
[![](https://goreportcard.com/badge/github.com/maistra/maistra-test-tool)](https://goreportcard.com/report/github.com/maistra/maistra-test-tool)


A testing tool for running Maistra tasks on AWS OpenShift 4.x cluster.

## Introduction

This project aims to automate the running Maistra tasks on an AWS OpenShift 4.x Cluster.

The testing follows [Istio Doc Tasks](https://istio.io/v1.4/docs/tasks/) and [Maistra Doc](https://maistra-1-1.maistra.io/).


## Versions

| Name      | Version       |
| --        | --            |
| OS        | Fedora 28+    |
| Golang    | 1.13+         |
| Python    | 3.7+          |

The host machine which triggers test scripts must have Golang and Python installed before running go tests.


## Testing Prerequisite

1. Maistra has been installed on an OpenShift OCP4 cluster.
2. Several test cases require nginx or mongoDB running on OCP4 and we need to configure additional scc permission for them after login as a cluster admin user.
   ```
   $ oc login -u kubeadmin -p [token] --server=[OCP API server]
   $ chmod +x scripts/setup_ocp_scc_anyuid.sh;
   $ scripts/setup_ocp_scc_anyuid.sh
   ```
3. Completed CLI login an OCP cluster as a regular user. Run `oc login -u [user] -p [token] --server=[OCP API server]` login command in a shell.
4. For the test case using JWT token, the system needs python3 installed to be able to run the python script.


## Testing
- All test cases and testdata are in the `tests` directory.
- The test cases include several changes for an OpenShift environment. Currently, those changes will not work in origin Kubernetes environments.
- To run all the test cases: `go test -timeout 3h -v`. It is required to use the `-timeout` flag. Otherwise, the go test by default will fall into panic after 10 minutes.
- In order to save results in a junit report, we can run a go test command with "github.com/jstemmer/go-junit-report", e.g.
    ```
    $ cd tests
    $ go get -u github.com/jstemmer/go-junit-report
    $ go test -timeout 3h -v 2>&1 | tee >(${GOPATH}/bin/go-junit-report > results.xml) test.log
    ```
- All case numbers are mapped in the `test_cases.go` file. Users can run a single test with the `-run [case number]` flag, e.g. `go -test -run 15 -timeout 1h -v`.
- The testdata `samples` and `samples_extend` are pulling from [Istio 1.4.6](https://github.com/istio/istio/releases/tag/1.4.6) and [Istio 1.4 Doc](https://archive.istio.io/v1.4/docs/tasks/).


## License

[Maistra OpenShift Test Tool](https://github.com/maistra/maistra-test-tool) is [Apache 2.0 licensed](https://github.com/maistra/maistra-test-tool/blob/development/LICENSE)
