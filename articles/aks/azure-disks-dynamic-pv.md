---
title: Azure Kubernetes Hizmetinde (AKS) Azure diskleriyle dinamik olarak kalıcı bir birim oluşturun ve kullanın
description: Azure Kubernetes Hizmetinde (AKS) Azure diskleriyle kalıcı bir birim oluşturmayı öğrenin
services: container-service
ms.topic: article
ms.date: 03/01/2019
ms.openlocfilehash: f16a6134b1d2065668952ea11c0cab7398a3559a
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617465"
---
# <a name="dynamically-create-and-use-a-persistent-volume-with-azure-disks-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Azure diskleriyle dinamik olarak kalıcı bir birim oluşturun ve kullanın

Kalıcı bir birim, Kubernetes bölmelerinde kullanılmak üzere sağlanmış bir depolama parçasını temsil eder. Kalıcı bir birim bir veya çok bölme tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Bu makalede, bir Azure Kubernetes Hizmeti (AKS) kümesinde tek bir bölme tarafından kullanılmak üzere Azure diskleri ile kalıcı birimler dinamik olarak nasıl oluşturulabileceğinizgösterilmektedir.

> [!NOTE]
> Azure diski yalnızca *Access modu* türü *ReadWriteOnce*ile monte edilebilir ve bu da onu AKS'de yalnızca tek bir bölmede kullanılabilir hale getirir. Kalıcı bir birimbirden çok bölmede paylaşmanız gerekiyorsa, [Azure Dosyaları'nı][azure-files-pvc]kullanın.

Kubernetes ciltleri hakkında daha fazla bilgi için [AKS'deki uygulamalar için Depolama seçeneklerine][concepts-storage]bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="built-in-storage-classes"></a>Yerleşik depolama sınıfları

Depolama sınıfı, depolama biriminin kalıcı bir hacimle dinamik olarak nasıl oluşturulduğunu tanımlamak için kullanılır. Kubernetes depolama sınıfları hakkında daha fazla bilgi için [Kubernetes Depolama Sınıfları'na][kubernetes-storage-classes]bakın.

Her AKS kümesi, her ikisi de Azure diskleriyle çalışacak şekilde yapılandırılan iki önceden oluşturulmuş depolama sınıfı içerir:

* *Varsayılan* depolama sınıfı standart bir Azure diski sağlar.
    * Standart depolama hdd'ler tarafından desteklenir ve hala performans gösterirken uygun maliyetli depolama sağlar. Standart diskler uygun maliyetli bir dev ve test iş yükü için idealdir.
* *Yönetilen premium* depolama sınıfı, birinci sınıf bir Azure diski sağlar.
    * Premium diskler SSD tabanlı, yüksek performanslı ve düşük gecikme süreli disk ile desteklenir. Üretim iş yükü çalıştıran VM'ler için son derece uygundur. Kümenizdeki AKS düğümleri premium depolamayı kullanıyorsa, *yönetilen premium* sınıfı seçin.
    
Bu varsayılan depolama sınıfları, oluşturulduktan sonra birim boyutunu güncelleştirmenize izin vermez. Bu yeteneği etkinleştirmek *için, izin veren VolumeExpansion: true* line'ı varsayılan depolama sınıflarından birine ekleyin veya kendi özel depolama sınıfınızı oluşturun. Komutu kullanarak varolan bir `kubectl edit sc` depolama sınıfını edinebilirsiniz. Depolama sınıfları ve kendi oluşturma hakkında daha fazla bilgi [için, AKS uygulamaları için Depolama seçenekleri][storage-class-concepts]bakın.

Önceden oluşturulmuş depolama sınıflarını görmek için [kubectl get sc][kubectl-get] komutunu kullanın. Aşağıdaki örnek, bir AKS kümesi içinde bulunan önceden oluşturuldepolama sınıflarını gösterir:

```console
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Kalıcı birim talepleri GiB'de belirtilir, ancak Azure yönetilen diskler belirli bir boyut için SKU tarafından faturalandırılır. Bu SK'ler S4 veya P4 diskler için 32GiB ile S80 veya P80 diskler için 32TiB arasında değişir (önizlemede). Premium yönetilen diskin iş ve IOPS performansı hem SKU'ya hem de AKS kümesindeki düğümlerin örnek boyutuna bağlıdır. Daha fazla bilgi için [bkz.][managed-disk-pricing-performance]

## <a name="create-a-persistent-volume-claim"></a>Kalıcı bir birim talebi oluşturma

Kalıcı bir birim talebi (PVC), depolama sınıfına dayalı depolamayı otomatik olarak sağlamak için kullanılır. Bu durumda, bir PVC standart veya premium Azure yönetilen disk oluşturmak için önceden oluşturulmuş depolama sınıflarından birini kullanabilir.

Adlı `azure-premium.yaml`bir dosya oluşturun ve aşağıdaki bildirimde kopyalayın. Talep, *ReadWriteOnce* erişimi ile *5 GB* boyutunda bir disk ister. `azure-managed-disk` *Yönetilen premium* depolama sınıfı depolama sınıfı olarak belirtilir.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

> [!TIP]
> Standart depolama alanı kullanan bir `storageClassName: default` disk oluşturmak için *yönetilen premium*yerine kullanın.

[Kubectl uygula][kubectl-apply] komutu ile kalıcı birim talebini oluşturun ve *azure-premium.yaml* dosyanızı belirtin:

```console
$ kubectl apply -f azure-premium.yaml

persistentvolumeclaim/azure-managed-disk created
```

## <a name="use-the-persistent-volume"></a>Kalıcı birimi kullanma

Kalıcı birim talebi oluşturulduktan ve disk başarıyla sağlandıktan sonra, diske erişim le birlikte bir bölme oluşturulabilir. Aşağıdaki bildirim, *Azure* diskini yola `/mnt/azure`monte etmek için azure yönetilen disk adlı kalıcı birim iddiasını kullanan temel bir NGINX bölmesi oluşturur. Windows Server kapsayıcıları için (şu anda AKS'de önizlemede), *'D:'* gibi Windows yolu kuralını kullanan bir *mountPath* belirtin.

Adlı `azure-pvc-disk.yaml`bir dosya oluşturun ve aşağıdaki bildirimde kopyalayın.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

Aşağıdaki örnekte gösterildiği gibi [kubectl uygula][kubectl-apply] komutu ile pod oluşturun:

```console
$ kubectl apply -f azure-pvc-disk.yaml

pod/mypod created
```

Artık Azure diskinizin dizine monte `/mnt/azure` edilmiş bir çalışan bölmeniz var. Bu `kubectl describe pod mypod`yapılandırma, aşağıdaki yoğunlaştırılmış örnekte gösterildiği gibi, pod'unuzu incelerken görülebilir:

```console
$ kubectl describe pod mypod

[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azure-managed-disk
    ReadOnly:   false
  default-token-smm2n:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-smm2n
    Optional:    false
[...]
Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              2m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  2m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-smm2n"
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "pvc-faf0f176-8b8d-11e8-923b-deb28c58d242"
[...]
```

## <a name="back-up-a-persistent-volume"></a>Kalıcı bir ses düzeyini yedekleme

Kalıcı biriminizdeki verileri yedeklemek için, birim için yönetilen diskin anlık görüntüsünü alın. Daha sonra geri yüklenen bir disk oluşturmak ve verileri geri yüklemek için bölmelere eklemek için bu anlık görüntü kullanabilirsiniz.

İlk olarak, `kubectl get pvc` *azure-managed-disk*adlı PVC için gibi komutu ile birim adı almak:

```console
$ kubectl get pvc azure-managed-disk

NAME                 STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
azure-managed-disk   Bound     pvc-faf0f176-8b8d-11e8-923b-deb28c58d242   5Gi        RWO            managed-premium   3m
```

Bu birim adı, temel Azure disk adını oluşturur. [Az disk listesi][az-disk-list] ile disk kimliği için sorgula ve aşağıdaki örnekte gösterildiği gibi PVC birim adınızı sağlayın:

```azurecli-interactive
$ az disk list --query '[].id | [?contains(@,`pvc-faf0f176-8b8d-11e8-923b-deb28c58d242`)]' -o tsv

/subscriptions/<guid>/resourceGroups/MC_MYRESOURCEGROUP_MYAKSCLUSTER_EASTUS/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

[Az anlık görüntü oluşturmak][az-snapshot-create]ile bir anlık görüntü diskoluşturmak için disk kimliğini kullanın. Aşağıdaki örnek, AKS kümesi *(MC_myResourceGroup_myAKSCluster_eastus)* ile aynı kaynak grubunda *pvcSnapshot* adlı bir anlık görüntü oluşturur. Aks kümesinin erişmediği kaynak gruplarında anlık görüntü oluşturur ve diskleri geri yüklerirseniz izin sorunlarıyla karşılaşabilirsiniz.

```azurecli-interactive
$ az snapshot create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name pvcSnapshot \
    --source /subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Diskinizdeki veri miktarına bağlı olarak anlık görüntünün oluşturulması birkaç dakika sürebilir.

## <a name="restore-and-use-a-snapshot"></a>Anlık görüntü geri yükleme ve kullanma

Diski geri yüklemek ve bir Kubernetes bölmesi ile kullanmak için, [az disk oluşturmak][az-disk-create]ile bir disk oluştururken bir kaynak olarak anlık görüntü kullanın. Bu işlem, özgün veri anlık görüntüsüne erişmeniz gerekiyorsa özgün kaynağı korur. Aşağıdaki örnek, *pvcSnapshot*adlı enstantaneden *geri yüklenen pvcGeri* adlı bir disk oluşturur:

```azurecli-interactive
az disk create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --source pvcSnapshot
```

Geri yüklenen diski bölmeyle kullanmak için, bildirimdeki diskin kimliğini belirtin. [Az disk show][az-disk-show] komutu ile disk kimliğini alın. Aşağıdaki örnek, önceki adımda oluşturulan *pvcRestored* için disk kimliğini alır:

```azurecli-interactive
az disk show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --query id -o tsv
```

Adlı `azure-restored.yaml` bir pod bildirimi oluşturun ve önceki adımda elde edilen disk URI'yi belirtin. Aşağıdaki örnek, */mnt/azure'da*bir birim olarak monte edilmiş geri yüklenen diskle birlikte temel bir NGINX web sunucusu oluşturur:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypodrestored
spec:
  containers:
  - name: mypodrestored
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      azureDisk:
        kind: Managed
        diskName: pvcRestored
        diskURI: /subscriptions/<guid>/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
```

Aşağıdaki örnekte gösterildiği gibi [kubectl uygula][kubectl-apply] komutu ile pod oluşturun:

```console
$ kubectl apply -f azure-restored.yaml

pod/mypodrestored created
```

Birim bilgilerini `kubectl describe pod mypodrestored` gösteren aşağıdaki yoğunlaştırılmış örnek gibi bölmenin ayrıntılarını görüntülemek için kullanabilirsiniz:

```console
$ kubectl describe pod mypodrestored

[...]
Volumes:
  volume:
    Type:         AzureDisk (an Azure Data Disk mount on the host and bind mount to the pod)
    DiskName:     pvcRestored
    DiskURI:      /subscriptions/19da35d3-9a1a-4f3b-9b9c-3c56ef409565/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
    Kind:         Managed
    FSType:       ext4
    CachingMode:  ReadWrite
    ReadOnly:     false
[...]
```

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi uygulamalar [için, AKS'deki depolama ve yedekleme ler için en iyi uygulamalara][operator-best-practices-storage]bakın.

Azure disklerini kullanarak Kubernetes kalıcı birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure diskler için Kubernetes eklentisi][azure-disk-volume]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/disks-types.md
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-snapshot-create]: /cli/azure/snapshot#az-snapshot-create
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-disk-show]: /cli/azure/disk#az-disk-show
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
[storage-class-concepts]: concepts-storage.md#storage-classes
