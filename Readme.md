# Sealed Secrets

## Source

    https://github.com/bitnami-labs/sealed-secrets

## Install with Argo CD

Use a central repository which links to all Apps which has to be deployed,
and add the App desciption there, or deploy the App for Sealed Secrets manually.
Find a sample in my [App of Apps Repo](https://github.com/wep4you/k8s-apps.git),
there is also a sample definition for [Sealed Secrets](https://github.com/wep4you/k8s-apps/blob/main/local/heimdal.yml)

## Install manual with kubectl

To add your own configuration, copy ```overlays/local``` to a new folder and change the files to your needs.
It's standard Kustomize format you can use.

    ```
    kubectl apply -k overlays/local
    ```

## CLI Tool

On Mac OS use Homebrew

    brew install kubeseal   

## Sealed Secret Usage

    # Create a json/yaml-encoded Secret somehow:
    # (note use of `--dry-run` - this is just a local file!)
    echo -n bar | kubectl create secret generic mysecret --dry-run=client --from-file=foo=/dev/stdin -o json >mysecret.json

    # This is the important bit:
    # (note default format is json!)
    kubeseal <mysecret.json >mysealedsecret.json

    # mysealedsecret.json is safe to upload to github, post to twitter,
    # etc.  Eventually:
    kubectl create -f mysealedsecret.json

    # Profit!
    kubectl get secret mysecret
