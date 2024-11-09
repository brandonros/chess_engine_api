# chess_bot

Rust chess bot via HTTP API + infrastructure exercise

## Technologies used

- Rust (smol, env_logger, http, miniserde)
- Lima (Apple Virtualization framework, Ubuntu GNU/Linux VM)
- K3s (Kubernetes cluster, workload orchestration)
- Traefik (ingress, SSL, load balancing, routing)
- Helm (package manager / YAML templating engine for Kubernetes)
- Prometheus, Grafana (metrics, dashboards, monitoring)
- cert-manager (automatedcertificates)
- docker-registry (container images)
- Kaniko (container builds)

## How to deploy VM

```shell
# dependencies
brew install kubectl lima helm

# provision VM
limactl start ./assets/ubuntu-k3s-vm.yaml

# configure kubectl
export KUBECONFIG="/Users/brandon/.lima/k3s/copied-from-guest/kubeconfig.yaml"

# trust CA
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain /Users/brandon/.lima/k3s/copied-from-guest/server-ca.crt
```

## How to deploy chess-bot

```shell
./scripts/setup.sh
./scripts/deploy.sh

# edit /etc/hosts
127.0.0.1 chess-bot.k3s.cluster.local

# get best move
curl --verbose -X POST -H 'Content-Type: application/json' https://chess-bot.k3s.cluster.local/chess/best-move -d '{
  "engine": "rustic",
  "depth": 6,
  "fen": "rnbqkbnr/pp1pppp1/8/2p4p/4P3/2P5/PP1P1PPP/RNBQKBNR w KQkq h6 0 3"
}'
```
