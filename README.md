# chess_engine_api

Rust JSON API over chess move scoring engine

## Technologies used

- Lima (Apple Virtualization framework, Debian GNU/Linux virtual machine)
- k3s (Kubernetes cluster, workload orchestration)
- Kong Mesh (service mesh, observability)
- Traefik (ingress, SSL, load balancing, routing)
- cert-manager + trust-manager (automated certificate management)
- docker-registry (stores and serves container images)
- Kaniko (builds container images)
- Helm (deployment package manager / YAML templating engine)
- smol (Rust async runtime)
- rustic (chess engine)
- ngrok (local-to-public endpoint mapper)

## TODO:

* backstage - service catalog
* kong - api gateway
* nats - messaging queue
* redis - caching
* postgres - database
* unleash - feature flags
* temporal - workflow engine
* minio - object storage

## How to use

```shell
# install dependencies
brew install kubectl lima helm

# configure kubectl
export KUBECONFIG="/Users/brandon/.lima/debian-k3s/copied-from-guest/kubeconfig.yaml"

# create VM + deploy infrastructure + build container
./scripts/deploy.sh

# get best move
while true
do
  sleep 1
  curl --verbose --fail -X POST -H 'Content-Type: application/json' https://chess-engine-api.debian-k3s/move/best -d '{
    "engine": "rustic",
    "depth": 6,
    "fen": "rnbqkbnr/pp1pppp1/8/2p4p/4P3/2P5/PP1P1PPP/RNBQKBNR w KQkq h6 0 3"
  }'
done

# cleanup
./scripts/cleanup.sh
```
