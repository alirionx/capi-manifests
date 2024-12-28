### Some CAPI on OpenStack -> Using Images from CAPI Image Builder
- https://github.com/kubernetes-sigs/cluster-api
- https://cluster-api-openstack.sigs.k8s.io/
- https://github.com/kubernetes-sigs/image-builder

<b><i>Fill in your values in capi-openstack-cloudinit.yaml</b></i>
```
clusterctl init --infrastructure openstack
kubectl apply -f capi-openstack-cloudinit.yaml
```

<br>


### Some CAPI on OpenStack -> Using Talos (K8s Linux)
- https://github.com/kubernetes-sigs/cluster-api
- https://cluster-api-openstack.sigs.k8s.io/
- https://www.talos.dev/v1.9/introduction/getting-started/
- https://github.com/siderolabs/cluster-api-control-plane-provider-talos
- https://github.com/siderolabs/cluster-api-bootstrap-provider-talos

<i>Fill in your values in capi-openstack-talos.yaml</i>
```
clusterctl init --bootstrap talos --control-plane talos --infrastructure openstack
kubectl apply -f capi-openstack-talos.yaml
```


<br>

### Post Config
- https://github.com/kubernetes/cloud-provider-openstack
- https://docs.tigera.io/calico/latest/about/

<i>Fill in your values in openstack-ccm-values.yaml and openstack-cinder-csi-values.yaml</i>
```
# not required 4 talos
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/refs/heads/master/manifests/calico.yaml

helm repo add cpo https://kubernetes.github.io/cloud-provider-openstack
helm repo update
helm upgrade --install openstack-ccm \
  cpo/openstack-cloud-controller-manager \
  --values cloud-provider-openstack/openstack-ccm-values.yaml \
  -n kube-system


helm upgrade --install cinder-csi \
  cpo/openstack-cinder-csi \
  --values cloud-provider-openstack/openstack-cinder-csi-values.yaml \
  -n kube-system
 

```