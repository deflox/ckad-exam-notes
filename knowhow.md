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

# `ENTRYPOINT` and `CMD` and how it is handled in kubernetes

In Docker, they have the following meaning:
* Only `ENTRYPOINT` defined: Cannot be overridden, running container with `docker run <container> <arg1> <arg2>` will append the `<arg1>` and `<arg2>` to the the executable that is defined as entrypoint
* Only `CMD` defined: Will be completely overridden when suplying arguments. When running `docker run <container> <arg1> <arg2>`, `<arg1>` must be some kind of executable otherwise it does not make sense

Source: https://stackoverflow.com/a/34245657

In Kubernetes:
| command | args | outcome                                                                                                                |
|---------|------|------------------------------------------------------------------------------------------------------------------------|
| Yes     | Yes  | Completely overriddes ENTRYPOINT and CMD and executes the command with the appended arguments                          |
| Yes     | No   | Completely overriddes ENTRYPOINT and CMD and executes the command                                                      |
| No      | Yes  | Args are appended to the ENTRYPOINT. If CMD is used instead of ENTRYPOINT, then the arguments act as their own command |
| No      | No   | Whatever is defined in the image is used                                                                               |

Source: https://stackoverflow.com/a/66078726