# validating-admission-policy-playground

[validating-admission-policy](https://kubernetes.io/docs/reference/access-authn-authz/validating-admission-policy/)

## Run the Example

```bash
oc new-project playground
oc label namespace playground vap=true

oc create -f samples/ValidatingAdmissionPolicy.yaml
oc create -f samples/ValidatingAdmissionPolicyBinding.yaml

oc create -f samples/deployment.yaml
oc create -f samples/deployment-no-label.yaml
oc create -f samples/deployment-fail.yaml
```

## Clean up

```bash
oc delete ValidatingAdmissionPolicy/deployments-cant-be-labeled
oc delete ValidatingAdmissionPolicyBinding/deployments-cant-be-labeled
oc delete project playground
```

## WARNING
Bad policies can degrade the cluster:

```
I0407 09:14:47.572877       1 event.go:377] Event(v1.ObjectReference{Kind:"", Namespace:"openshift-machine-config-operator", Name:"machine-config", UID:"5465f007-71da-4061-9339-b5acd4ff56af", APIVersion:"", ResourceVersion:"", FieldPath:""}):
type: 'Warning' reason: 'OperatorDegraded: MachineConfigControllerFailed' Failed to resync 4.18.6 because: deployments.apps "machine-config-controller" is forbidden:
ValidatingAdmissionPolicy 'deployments-cant-be-labeled' with binding 'deployments-cant-be-labeled' denied request: The 'sidecar.istio.io/inject' label cannot be added to Deployment
```

If you start to see issues, check the machine-config-operator:

```bash
oc logs deployment/machine-config-operator -n openshift-machine-config-operator -f
```
