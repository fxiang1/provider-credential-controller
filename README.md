# Provider-credential-controller

[![License](https://img.shields.io/:license-apache-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0.html)

## What is provider-credential-controller

With `provider-credential-controller`, your cluster secrets will be automatically updated when making changes to the Provider Crednetial secrets.

Go to the [Contributing guide](CONTRIBUTING.md) to learn how to get involved.

## Getting started

- ### Steps for development: 

  - Compile the code by running `make compile`
  - Run the controller manually `./build/_output/provider-credential-controller`
  - Run the go file manually `go run ./cmd/manager/main.go`


  - Push an image to your repository:
    ```bash
    export VERSION=0.1
    export REPO_URL=quay.io/MY_ORGANIZATION_OR_USERNAME

    make push
    ```


- ### Steps for deployment:

  - Connect to the OpenShift cluster acting as the hub for Open Cluster Management
    ```bash
    oc apply -k deploy/controller
    ```
    - Even though this controller deploys as a single pod, it uses leader election to make sure only one instance is ever running.
    - Even if the controller is interupted while updating secrets, when it restarts, it will continue the process until all copied secrets are updated with the new values from the Provider Credential secret.
  - To launch from the command line:
    ```bash
    go run ./cmd/manager
    ```


- ### Steps for testing:

  - Running unit tests:
    ```bash
    make unit-tests
    ```

  - Running scale testing (3000 copied secrets)
    - Connect to an OpenShift cluster
    - Make sure either the controller is deployed via the provided kubernetes manifest, or launched from the command line
      ```bash
      # Create namespace
      oc new-project providers

      make scale-up   # This creates a fake Ansible Provider Secret, and makes 3000 copies

      make scale-test # This makes FOUR token changes to the Provider secret without waiting

      make scale-down # Removes a fake Ansible Provider Secret and deletes 3000 copies
      ```
    - This test executes a sequence of four token updates, not waiting for the 3000 copies to be reconciled. This validates that we do not lose track of the Provider secret updates, even when there is a processing delay in reconciling each copied secret.


- Check the [Security guide](SECURITY.md) if you need to report a security issue.


## References

- The `provider-credential-controller` is part of the `open-cluster-management` community. For more information, visit: [open-cluster-management.io](https://open-cluster-management.io).

