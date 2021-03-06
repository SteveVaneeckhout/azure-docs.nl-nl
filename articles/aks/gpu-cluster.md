---
title: GPU's op Azure Kubernetes Service (AKS) gebruiken
description: Informatie over het gebruik van GPU's voor high performance computing- of grafisch intensieve werkbelastingen op Azure Kubernetes Service (AKS)
services: container-service
author: lachie83
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 10/25/2018
ms.author: laevenso
ms.openlocfilehash: db9d42cca4bc327d216229f45f5fa1742f70fb9a
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138806"
---
# <a name="use-gpus-for-compute-intensive-workloads-on-azure-kubernetes-service-aks"></a>GPU's gebruiken voor rekenintensieve workloads in Azure Kubernetes Service (AKS)

Grafische verwerkingseenheden (GPU's) worden vaak gebruikt voor rekenintensieve workloads, zoals afbeeldingen en visualisatie werkbelastingen. AKS ondersteunt het maken van pools met GPU knooppunt om uit te voeren van deze rekenintensieve workloads in Kubernetes. Zie voor meer informatie over beschikbare virtuele machines met GPU [GPU VM-grootten in Azure geoptimaliseerd][gpu-skus]. Voor de AKS-knooppunten, raden we aan een minimumgrootte van *Standard_NC6*.

> [!NOTE]
> Virtuele machines met GPU bevat speciale hardware die is onderworpen aan hogere beschikbaarheid van prijzen en regio. Zie voor meer informatie de [prijzen] [ azure-pricing] hulpprogramma en [beschikbaarheid in regio][azure-availability].

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u hebt een bestaand AKS-cluster met knooppunten die ondersteuning bieden voor GPU's. Uw AKS-cluster moet Kubernetes 1,10 of hoger uitvoeren. Als u een AKS-cluster dat aan deze vereisten voldoet, Zie de eerste sectie van dit artikel om [maken van een AKS-cluster](#create-an-aks-cluster).

U ook moet de Azure CLI versie 2.0.49 of later geïnstalleerd en geconfigureerd. Voer `az --version` de versie te vinden. Als u wilt installeren of upgraden, Zie [Azure CLI installeren][install-azure-cli].

## <a name="create-an-aks-cluster"></a>Een AKS-cluster maken

Als u een AKS-cluster nodig hebt die voldoet aan de minimale vereisten (knooppunt met GPU en Kubernetes 1,10 of hoger), voer de volgende stappen uit. Als u al een AKS-cluster dat aan deze vereisten voldoet, gaat u naar de volgende sectie.

Maak eerst een resourcegroep voor de cluster met de [az-groep maken] [ az-group-create] opdracht. Het volgende voorbeeld wordt een Resourcegroepnaam *myResourceGroup* in de *eastus* regio:

```azurecli
az group create --name myResourceGroup --location eastus
```

Maak nu een AKS-cluster met de [az aks maken] [ az-aks-create] opdracht. Het volgende voorbeeld wordt een cluster met één knooppunt van de grootte van `Standard_NC6`, en wordt Kubernetes versie 1.10.8 uitgevoerd:

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-vm-size Standard_NC6 \
    --node-count 1 \
    --kubernetes-version 1.10.8
```

Haal de referenties voor uw AKS-cluster met de [az aks get-credentials] [ az-aks-get-credentials] opdracht:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="confirm-that-gpus-are-schedulable"></a>Controleer of GPU's zijn Planbare

Bevestig dat GPU's in Kubernetes Planbare zijn met uw AKS-cluster gemaakt. Lijst eerst de knooppunten in uw cluster met de [kubectl ophalen knooppunten] [ kubectl-get] opdracht:

```
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-18821093-0   Ready    agent   6m    v1.10.8
```

Nu gebruik van de [kubectl beschrijven knooppunt] [ kubectl-describe] opdracht om te bevestigen dat de GPU's Planbare zijn. Onder de *capaciteit* sectie, als de GPU moet vermelden `nvidia.com/gpu:  1`. Als u de GPU's niet ziet, raadpleegt u de [oplossen GPU beschikbaarheid](#troubleshoot-gpu-availability) sectie.

Het volgende verkorte voorbeeld laat zien dat een GPU die beschikbaar op het knooppunt met de naam is *aks-nodepool1-18821093-0*:

```
$ kubectl describe node aks-nodepool1-18821093-0

Name:               aks-nodepool1-18821093-0
Roles:              agent
Labels:             accelerator=nvidia

[...]

Capacity:
 cpu:                6
 ephemeral-storage:  30428648Ki
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             57713824Ki
 nvidia.com/gpu:     1
 pods:               110
Allocatable:
 cpu:                5940m
 ephemeral-storage:  28043041951
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             53417120Ki
 nvidia.com/gpu:     1
 pods:               110
System Info:
 Machine ID:                 688e083d19554d4a9563bd138f4ca98b
 System UUID:                08162568-B987-A84D-8865-98D6EFC64B32
 Boot ID:                    7b440249-8a96-42eb-950f-08c9a3c530b7
 Kernel Version:             4.15.0-1023-azure
 OS Image:                   Ubuntu 16.04.5 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://1.13.1
 Kubelet Version:            v1.10.8
 Kube-Proxy Version:         v1.10.8
PodCIDR:                     10.244.0.0/24
ProviderID:                  azure:///subscriptions/19da35d3-9a1a-4f3b-9b9c-3c56ef409565/resourceGroups/MC_myGPUCluster_myGPUCluster_eastus/providers/Microsoft.Compute/virtualMachines/aks-nodepool1-18821093-0
Non-terminated Pods:         (9 in total)
  Namespace                  Name                                    CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ---------                  ----                                    ------------  ----------  ---------------  -------------
  gpu-resources              nvidia-device-plugin-9cfcf              0 (0%)        0 (0%)      0 (0%)           0 (0%)

[...]
```

## <a name="run-a-gpu-enabled-workload"></a>Een workload met GPU uitvoeren

Als u wilt de GPU in actie zien, plant u een workload met GPU met de juiste resourceaanvraag. In dit voorbeeld gaan we voeren een [Tensorflow](https://www.tensorflow.org/versions/r1.1/get_started/mnist/beginners) taak op basis van de [MNIST gegevensset](http://yann.lecun.com/exdb/mnist/).

Maak een bestand met de naam *voorbeelden-tf-mnist-demo.yaml* en plak de volgende YAML-manifest. De volgende taak manifest bevat een resourcelimiet van `nvidia.com/gpu: 1`:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  labels:
    app: samples-tf-mnist-demo
  name: samples-tf-mnist-demo
spec:
  template:
    metadata:
      labels:
        app: samples-tf-mnist-demo
    spec:
      containers:
      - name: samples-tf-mnist-demo
        image: microsoft/samples-tf-mnist-demo:gpu
        args: ["--max_steps", "500"]
        imagePullPolicy: IfNotPresent
        resources:
          limits:
           nvidia.com/gpu: 1
      restartPolicy: OnFailure
```

Gebruik de [kubectl toepassen] [ kubectl-apply] opdracht wordt uitgevoerd. Met deze opdracht parseert het manifestbestand en maakt de gedefinieerde Kubernetes-objecten:

```console
kubectl apply -f samples-tf-mnist-demo.yaml
```

## <a name="view-the-status-and-output-of-the-gpu-enabled-workload"></a>De status en de uitvoer van de werkbelasting met GPU weergeven

De voortgang van de taak met behulp van de [kubectl taken ophalen] [ kubectl-get] opdracht met de `--watch` argument. Het kan enkele minuten duren voor eerste pull de installatiekopie en verwerken van de gegevensset. Wanneer de *VOLTOOIINGEN* kolom toont de *1/1*, de taak is voltooid:

```
$ kubectl get jobs samples-tf-mnist-demo --watch

NAME                    COMPLETIONS   DURATION   AGE

samples-tf-mnist-demo   0/1           3m29s      3m29s
samples-tf-mnist-demo   1/1   3m10s   3m36s
```

Als u wilt de uitvoer van de werkbelasting met GPU bekijkt, moet u eerst de naam van het gehele product met ontvangen de [kubectl ophalen schillen] [ kubectl-get] opdracht:

```
$ kubectl get pods --selector app=samples-tf-mnist-demo

NAME                          READY   STATUS      RESTARTS   AGE
samples-tf-mnist-demo-smnr6   0/1     Completed   0          3m
```

Gebruik nu de [kubectl logboeken] [ kubectl-logs] opdracht om de schil logboeken weer te geven. De volgende logboeken van de voorbeeld-schil bevestigen dat het juiste GPU-apparaat is gedetecteerd, `Tesla K80`. Geef de naam op voor uw eigen pod:

```
$ kubectl logs samples-tf-mnist-demo-smnr6

2018-10-25 18:31:10.155010: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2018-10-25 18:31:10.305937: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: ccb6:00:00.0
totalMemory: 11.92GiB freeMemory: 11.85GiB
2018-10-25 18:31:10.305981: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: ccb6:00:00.0, compute capability: 3.7)
2018-10-25 18:31:14.941723: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.097
Accuracy at step 10: 0.6993
Accuracy at step 20: 0.8208
Accuracy at step 30: 0.8594
Accuracy at step 40: 0.8685
Accuracy at step 50: 0.8864
Accuracy at step 60: 0.901
Accuracy at step 70: 0.905
Accuracy at step 80: 0.9103
Accuracy at step 90: 0.9126
Adding run metadata for 99
Accuracy at step 100: 0.9176
Accuracy at step 110: 0.9149
Accuracy at step 120: 0.9187
Accuracy at step 130: 0.9253
Accuracy at step 140: 0.9252
Accuracy at step 150: 0.9266
Accuracy at step 160: 0.9255
Accuracy at step 170: 0.9267
Accuracy at step 180: 0.9257
Accuracy at step 190: 0.9309
Adding run metadata for 199
Accuracy at step 200: 0.9272
Accuracy at step 210: 0.9321
Accuracy at step 220: 0.9343
Accuracy at step 230: 0.9388
Accuracy at step 240: 0.9408
Accuracy at step 250: 0.9394
Accuracy at step 260: 0.9412
Accuracy at step 270: 0.9422
Accuracy at step 280: 0.9436
Accuracy at step 290: 0.9411
Adding run metadata for 299
Accuracy at step 300: 0.9426
Accuracy at step 310: 0.9466
Accuracy at step 320: 0.9458
Accuracy at step 330: 0.9407
Accuracy at step 340: 0.9445
Accuracy at step 350: 0.9486
Accuracy at step 360: 0.9475
Accuracy at step 370: 0.948
Accuracy at step 380: 0.9516
Accuracy at step 390: 0.9534
Adding run metadata for 399
Accuracy at step 400: 0.9501
Accuracy at step 410: 0.9552
Accuracy at step 420: 0.9535
Accuracy at step 430: 0.9545
Accuracy at step 440: 0.9533
Accuracy at step 450: 0.9526
Accuracy at step 460: 0.9566
Accuracy at step 470: 0.9547
Accuracy at step 480: 0.9548
Accuracy at step 490: 0.9545
Adding run metadata for 499
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt verwijderen van de gekoppelde Kubernetes-objecten in dit artikel hebt gemaakt, gebruikt u de [kubectl verwijderen van de taak] [ kubectl delete] opdracht als volgt:

```console
kubectl delete jobs samples-tf-mnist-demo
```

## <a name="troubleshoot-gpu-availability"></a>GPU-beschikbaarheid oplossen

Als er geen GPU's als deze beschikbaar is op uw knooppunten, moet u mogelijk een DaemonSet voor de invoegtoepassing nVidia-apparaat implementeren. Deze DaemonSet uitvoert pod op elk knooppunt voor de vereiste stuurprogramma's voor de GPU's.

Maak eerst een naamruimte met de [kubectl-naamruimte maken] [ kubectl-create] opdracht, zoals *gpu-resources*:

```console
kubectl create namespace gpu-resources
```

Maak een bestand met de naam *nvidia-apparaat-invoegtoepassing-ds.yaml* en plak de volgende YAML-manifest. Update de `image: nvidia/k8s-device-plugin:1.10` halverwege omlaag het manifest overeenkomen met uw Kubernetes-versie. Bijvoorbeeld, als uw AKS-cluster wordt uitgevoerd Kubernetes versie 1.11, het label bijwerken `image: nvidia/k8s-device-plugin:1.11`.

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  labels:
    kubernetes.io/cluster-service: "true"
  name: nvidia-device-plugin
  namespace: gpu-resources
spec:
  template:
    metadata:
      # Mark this pod as a critical add-on; when enabled, the critical add-on scheduler
      # reserves resources for critical add-on pods so that they can be rescheduled after
      # a failure.  This annotation works in tandem with the toleration below.
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ""
      labels:
        name: nvidia-device-plugin-ds
    spec:
      tolerations:
      # Allow this pod to be rescheduled while the node is in "critical add-ons only" mode.
      # This, along with the annotation above marks this pod as a critical add-on.
      - key: CriticalAddonsOnly
        operator: Exists
      containers:
      - image: nvidia/k8s-device-plugin:1.10 # Update this tag to match your Kubernetes version
        name: nvidia-device-plugin-ctr
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop: ["ALL"]
        volumeMounts:
          - name: device-plugin
            mountPath: /var/lib/kubelet/device-plugins
      volumes:
        - name: device-plugin
          hostPath:
            path: /var/lib/kubelet/device-plugins
      nodeSelector:
        beta.kubernetes.io/os: linux
        accelerator: nvidia
```

Gebruik nu de [kubectl toepassen] [ kubectl-apply] opdracht voor het maken van de DaemonSet:

```
$ kubectl apply -f nvidia-device-plugin-ds.yaml

daemonset "nvidia-device-plugin" created
```

Voer de [kubectl beschrijven knooppunt] [ kubectl-describe] opdracht opnieuw uit om te controleren of de GPU is nu beschikbaar op het knooppunt.

## <a name="next-steps"></a>Volgende stappen

Zie het Apache Spark-taken uitvoeren, [uitvoeren Apache Spark-taken in AKS][aks-spark].

Zie voor meer informatie over het uitvoeren van machine learning (ML) workloads in Kubernetes [Kubeflow Labs][kubeflow-labs].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubeflow-labs]: https://github.com/Azure/kubeflow-labs
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[azure-pricing]: https://azure.microsoft.com/pricing/
[azure-availability]: https://azure.microsoft.com/global-infrastructure/services/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-spark]: spark-job.md
[gpu-skus]: ../virtual-machines/linux/sizes-gpu.md
[install-azure-cli]: /cli/azure/install-azure-cli
