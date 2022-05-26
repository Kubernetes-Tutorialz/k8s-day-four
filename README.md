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

1.1 apenas para nivel de entendimento dos volumes desse POD, o bloco de volumes esta sendo relacionado ao volume de fato:

```yml
 volumes:
  - name: giropops-dir
    emptyDir: {}
```

1.2 para a parte do `volumeMounts` e a forma como eu estou montando esse volume dentro do container.

```yml
volumeMounts:
    - mountPath: /giropops
      name: giropops-dir
```


