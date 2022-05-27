## Volume EmptyDir

Basicamente esse tipo de volume esta relacionado a um POD, ele sempre se inicia vazio, e criado junto com o POD. Lembrando que como os recursos dentro de um POD sao
compartilhados, todos os containers desse POD podem usar esse modelo de volume. Nao existe persistencia de dados, se remover esse POD os dados ali salvos sao removidos.

1.  Vamos criar nosso `YML` de teste para que possamos exemplificar nosso exemplo:

```yml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: default
spec:
  containers:
  - image: busybox
    name: busy
    command:
      - sleep
      - "3600"
    volumeMounts:
    - mountPath: /giropops
      name: giropops-dir
  volumes:
  - name: giropops-dir
    emptyDir: {}
```

1.1 apenas para nivel de entendimento dos volumes desse POD, o bloco de volumes esta sendo relacionado ao volume de fato (criando o  volume dentro do container)

```yml
 volumes:
  - name: giropops-dir
    emptyDir: {}
```

1.2 para a parte do `volumeMounts` e a forma como eu estou montando esse volume dentro do container (montando o volume dentro do container)

```yml
volumeMounts:
    - mountPath: /giropops
      name: giropops-dir
```

1.3 Para que possamos criar esse POD:

`# kubectl create -f pod_emptydir.yml`

1.4 Agora vamos rodar o comando de describe:

`# kubectl describe pod -n default busybox`

```bash 
Name:         busybox
Namespace:    default
Priority:     0
Node:         k8snode02/192.168.0.132        
Start Time:   Fri, 11 Mar 2022 10:11:58 -0300
Labels:       <none>
Annotations:  <none>
Status:       Running
IP:           10.47.0.1
IPs:
  IP:  10.47.0.1
Containers:
  busy:
    Container ID:  docker://4e0e21a0a2f411f682301014ac19be6fef90031f6d2945f063af63a592a3ed93
    Image:         busybox
    Image ID:      docker-pullable://busybox@sha256:547706ea975b810cbddc457dcfd2fcc2bd935da585c0ae72a64a341863582f1e
    Port:          <none>
    Host Port:     <none>
    Command:
      sleep
      3600
    State:          Running
      Started:      Fri, 11 Mar 2022 10:12:02 -0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /giropops from giropops-dir (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-x8s8n (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  giropops-dir:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  kube-api-access-x8s8n:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  5m26s  default-scheduler  Successfully assigned default/busybox to k8snode02
  Normal  Pulling    16h    kubelet            Pulling image "busybox"
  Normal  Pulled     16h    kubelet            Successfully pulled image "busybox" in 2.644776386s
  Normal  Created    16h    kubelet            Created container busy
  Normal  Started    16h    kubelet            Started container busy
```





