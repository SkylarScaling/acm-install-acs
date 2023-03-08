# acm-install-acs
Deploy ACS to ACM hub and managed clusters using ACM policygenerator

Notes:
- User needs to be added to open-cluster-management:subscription-admin ClusterRoleBinding


    kind: ClusterRoleBinding  
    apiVersion: rbac.authorization.k8s.io/v1  
    metadata:  
      name: 'open-cluster-management:subscription-admin'
    subjects:
      - kind: User
        apiGroup: rbac.authorization.k8s.io
        name: admin
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: 'open-cluster-management:subscription-admin'

- Add user to system:image-puller role for project


    kind: ClusterRoleBinding  
    apiVersion: rbac.authorization.k8s.io/v1
    metadata:
      name: sky-image-pull-binding
    subjects:
      - kind: User
        apiGroup: rbac.authorization.k8s.io
        name: admin
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: 'system:image-puller'

