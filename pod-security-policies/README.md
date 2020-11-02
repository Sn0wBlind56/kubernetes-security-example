# Pod Security Policies

## Enabling the PodSecurityPolicy Admission Controller

The static pod configuration for the kube-apiserver pod can be found under `/etc/kubernetes/manifests/kube-apiserver.yaml`.
In this file, there should be a line starting with `--enable-admission-plugins`. This argument provides a comma-separated
list of enabled admission controllers. To enable the PodSecurityPolicy functionality, add `PodSecurityPolicy` to this list.
Here's an example:

```
--enable-admission-plugins=NodeRestriction,PodSecurityPolicy
```

Saving this file will cause the Kubelet to automatically recreate the static pod. You'll notice that the kube-apiserver pod
is gone from the pod overview when requesting it from the Kubernetes API server. This is because the Kubelet can no longer
create a mirror pod for the static pod it just recreated. It no longer has permissions to do so, as it isn't allowed to use
any pod security policy. By default, when the user (or the Kubelet) has no permission to use at least 1 pod security policy,
the request will be rejected by the Kubernetes API server. Logging about this error can be found through the command below.

```
journalctl -u kubelet
```
