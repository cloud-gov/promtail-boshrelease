# Promtail BOSH Release

This is a [BOSH](http://bosh.io/) release for the [Promtail](https://github.com/grafana/loki/tree/master/cmd/promtail) for Linux based stemcells.

It is intented to be deployed as a [BOSH Addon](http://bosh.io/docs/runtime-config.html#addons) and alongside the [Loki BOSH Release](https://github.com/bosh-loki/loki-boshrelease).

## Usage

To use this BOSH release, first upload it to your BOSH:

```
export BOSH_ENVIRONMENT=<name>
bosh upload-release 
```

Then create a runtime configuration file:

```
releases:
  - name: promtail
    version: 0.0.1

addons:
  - name: promtail
    jobs:
      - name: promtail
        release: promtail
        consumes:
          loki: {from: loki, deployment: loki}
    include:
      stemcell:
        - os: ubuntu-trusty
        - os: ubuntu-xenial
    properties: {}
```

Now you can update your [BOSH Runtime Config](http://bosh.io/docs/runtime-config.html) with the previously created file:

```
bosh update-config --name promtail --type runtime <your runtime-config.yaml file location>
```

Once runtime config is updated it will applied to all new deployments (the existing deployments will be considered outdated and they will be update when they are deployed again).

## License

Apache License 2.0, see [LICENSE](https://github.com/bosh-loki/promtail-boshrelease/blob/master/LICENSE).
