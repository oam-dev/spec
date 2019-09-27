# Identity scope

The identity scope provides identities in components when authenticating to any service. The identities and its credentials are supplied and managed by an external identity provider.

## Example

This is an example of a identity scope definition.

```yaml
apiVersion: core.hydra.io/v1alpha1
kind: ApplicationScope
metadata:
  name: myIdentityScope
  annotations:
    version: v1.0.0
    description: "The production configuration for Corp CMS"
spec:
  type: extension.hydra.io/v1.IdentityScope
  allowComponentOverlap: true
  parameters:
    - name: IdentityProvider
      description: the provider of identity for the scope
      type: string
      required: Y
```
