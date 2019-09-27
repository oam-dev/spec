# Resource Quota scope

The resource quota scope sets resource quotas on a group of components. Resources include CPU, memory, storage, etc. Setting resource quotas for a scope applies across all components within the scope; in other words, the total resource consumption for all components within a scope cannot exceed a certain value.

## Example

This is an example of a resource quota scope definition.

```yaml
apiVersion: core.hydra.io/v1alpha1
kind: ApplicationScope
metadata:
  name: myResourceQuotas
  annotations:
    version: v1.0.0
    description: "The production configuration for Corp CMS"
spec:
  type: extension.hydra.io/v1.ResourceQuotaScope
  allowComponentOverlap: false
  parameters:
    - name: CPU
      description: maximum CPU to be consumed by this scope
      type: double
      required: Y
    - name: Memory
      description: maximum memory to be consumed by this scope
      type: double
      required: Y
```
