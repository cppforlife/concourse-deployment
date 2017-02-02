# concourse-deployment

- Requires new [BOSH CLI v0.0.147+](https://github.com/cloudfoundry/bosh-cli)

Modify `local.sh` or run following:

```
$ rm -f creds.yml

$ bosh create-env concourse.yml \
  --state ./state.json \
  -o ./virtualbox/cpi.yml \
  -o ./virtualbox/outbound-network.yml \
  --vars-store ./creds.yml \
  -v internal_ip=192.168.99.6 \
  -v internal_gw=192.168.99.1 \
  -v internal_cidr=192.168.99.1/24 \
  -v network_name=vboxnet0 \
  -v outbound_network_name=NatNetwork
```

Log into deployed Concourse (grab fly binary from `https://192.168.99.6`):

```
$ fly -t local login -c https://192.168.99.6 \
  --username admin \
  --password `bosh int creds.yml --path /ui_password` \
  --ca-cert <(bosh int creds.yml --path /atc_ssl/ca)

$ fly -t local pipelines
```
