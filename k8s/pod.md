# pod

## lifecycle

### create

创建pod时发生了啥

1. The Pod is stored in etcd.
2. The scheduler assigns a Node. It writes the node in etcd.
3. The kubelet is notified of a new and scheduled Pod.
4. The kubelet delegates creating the container to the Container Runtime Interface (CRI).
5. The kubelet delegates attaching the container to the Container Network Interface (CNI).
6. The kubelet delegates mounting volumes in the container to the Container Storage Interface (CSI).
7. The Container Network Interface assigns an IP address.
8. The kubelet reports the IP address to the control plane.
9. The IP address is stored in etcd.

在有对应service的情况下

1. The kubelet waits for a successful Readiness probe.
2. All relevant Endpoints (objects) are notified of the change.
3. The Endpoints add a new endpoint (IP address + port pair) to their list.
4. Kube-proxy is notified of the Endpoint change. Kube-proxy updates the iptables rules on every node.
5. The Ingress controller is notified of the Endpoint change. The controller routes traffic to the new IP addresses.
6. CoreDNS is notified of the Endpoint change. If the Service is of type Headless, the DNS entry is updated.
7. The cloud provider is notified of the Endpoint change. If the Service is of type: LoadBalancer, the new endpoint are configured as part of the load balancer pool.
8. Any service mesh installed in the cluster is notified of the Endpoint change.
9. Any other operator subscribed to Endpoint changes is notified, too.