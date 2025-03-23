# Why in for NetworkPolicy can I use either `-` or not?

* `podSelector` and `namespaceSelector` and combined as an AND rule and
* `ipBlock` is combined with the other block as an OR rule
* Every block could theoretically contain all three attributes and also not also all three attributes

```yaml
spec:
  ingress:
  - from:
    - podSelector:
      ...
      namespaceSelector:
      ...
    - ipBlock:
      ...
```