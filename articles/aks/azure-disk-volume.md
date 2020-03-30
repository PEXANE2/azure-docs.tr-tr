---
title: Azure Kubernetes Hizmeti'nde (AKS) bölmeler için statik bir birim oluşturma
description: Azure Kubernetes Hizmetinde (AKS) bir bölmeyle kullanılmak üzere Azure diskleriyle nasıl el ile bir birim oluşturabilirsiniz öğrenin
services: container-service
ms.topic: article
ms.date: 03/01/2019
ms.openlocfilehash: 17795ae696c0d710f099a5c21aa754fc925953ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80047949"
---
# <a name="manually-create-and-use-a-volume-with-azure-disks-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Azure diskleriyle el ile bir birim oluşturma ve kullanma

Kapsayıcı tabanlı uygulamaların genellikle verilere harici bir veri hacminde erişmeleri ve verileri devam etmesi gerekir. Tek bir bölmenin depolama alanına erişmesi gerekiyorsa, uygulama kullanımı için yerel bir birim sunmak için Azure disklerini kullanabilirsiniz. Bu makalede, bir Azure diskinin el ile nasıl oluşturulup AKS'deki bir bölmeye nasıl ekselreceniz gösterilmektedir.

> [!NOTE]
> Azure diski aynı anda yalnızca tek bir bölmeye monte edilebilir. Kalıcı bir birimbirden çok bölmede paylaşmanız gerekiyorsa, [Azure Dosyaları'nı][azure-files-volume]kullanın.

Kubernetes ciltleri hakkında daha fazla bilgi için [AKS'deki uygulamalar için Depolama seçeneklerine][concepts-storage]bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="create-an-azure-disk"></a>Azure diski oluşturma

AKS ile kullanılmak üzere bir Azure diski oluşturduğunuzda, **düğüm** kaynak grubunda disk kaynağı oluşturabilirsiniz. Bu yaklaşım, AKS kümesinin disk kaynağına erişmesine ve yönetilmesine olanak tanır. Bunun yerine diski ayrı bir kaynak grubunda oluşturursanız, kümeniz `Contributor` için Azure Kubernetes Hizmeti (AKS) hizmet ilkesini diskin kaynak grubuna vermeniz gerekir. Alternatif olarak, hizmet sorumlusu yerine izinler için yönetilen kimlik atanan sistemi kullanabilirsiniz. Daha fazla bilgi için [bkz.](use-managed-identity.md)

Bu makale için, düğüm kaynak grubunda disk oluşturun. İlk olarak, [az aks show][az-aks-show] komutu ile `--query nodeResourceGroup` kaynak grup adını alın ve sorgu parametresi ekleyin. Aşağıdaki örnek, *myResourceGroup*kaynak grubu adı aks küme adı *myAKSCluster* için düğüm kaynak grubu alır:

```azurecli-interactive
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Şimdi az disk [oluşturma][az-disk-create] komutunu kullanarak bir disk oluşturun. Önceki komutta elde edilen düğüm kaynak grubu adını ve ardından *myAKSDisk*gibi disk kaynağının adını belirtin. Aşağıdaki örnekte *20*GiB disk oluşturulur ve oluşturulduktan sonra diskin kimliğini çıkar. Windows Server kapsayıcılarında (şu anda AKS'de önizlemede) kullanılmak `--os-type windows` üzere bir disk oluşturmanız gerekiyorsa, diski doğru biçimlendirmek için parametreyi ekleyin.

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk \
  --size-gb 20 \
  --query id --output tsv
```

> [!NOTE]
> Azure diskler Belirli bir boyut için SKU tarafından faturalandırılır. Bu SK'ler S4 veya P4 diskler için 32GiB ile S80 veya P80 diskler için 32TiB arasında değişir (önizlemede). Premium yönetilen diskin iş ve IOPS performansı hem SKU'ya hem de AKS kümesindeki düğümlerin örnek boyutuna bağlıdır. [Yönetilen Disklerin Fiyatlandırması ve Performansı'na][managed-disk-pricing-performance]bakın.

Aşağıdaki örnek çıktıda gösterildiği gibi, komut başarıyla tamamlandıktan sonra disk kaynak kimliği görüntülenir. Bu disk kimliği, bir sonraki adımda diski takmak için kullanılır.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

## <a name="mount-disk-as-volume"></a>Ses olarak diski takma

Azure diskini bölmenize monte etmek için, birim hacmi kapsayıcı spec'inde yapılandırın. Aşağıdaki içeriklerle birlikte yeni `azure-disk-pod.yaml` bir dosya oluşturun. Önceki `diskName` adımda oluşturulan diskin adı ile `diskURI` ve disk çıktısında gösterilen disk kimliği ile güncelleştirme komutu oluşturun. İstenirse, Azure diskinin `mountPath`bölmeye monte edildiği yol olan yolu güncelleştirin. Windows Server kapsayıcıları için (şu anda AKS'de önizlemede), *'D:'* gibi Windows yolu kuralını kullanan bir *mountPath* belirtin.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.5
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
  volumes:
      - name: azure
        azureDisk:
          kind: Managed
          diskName: myAKSDisk
          diskURI: /subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

Bölmeyi `kubectl` oluşturmak için komutu kullanın.

```console
kubectl apply -f azure-disk-pod.yaml
```

Artık azure diskine `/mnt/azure`monte edilmiş çalışan bir bölmeniz var. Diskin `kubectl describe pod mypod` başarıyla monte edilebilmektedir doğrulamak için kullanabilirsiniz. Aşağıdaki yoğunlaştırılmış örnek çıktı, kapsayıcıya monte edilen hacmi gösterir:

```
[...]
Volumes:
  azure:
    Type:         AzureDisk (an Azure Data Disk mount on the host and bind mount to the pod)
    DiskName:     myAKSDisk
    DiskURI:      /subscriptions/<subscriptionID/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
    Kind:         Managed
    FSType:       ext4
    CachingMode:  ReadWrite
    ReadOnly:     false
  default-token-z5sd7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z5sd7
    Optional:    false
[...]
Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              1m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-z5sd7"
  Normal  SuccessfulMountVolume  41s   kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "azure"
[...]
```

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi uygulamalar [için, AKS'deki depolama ve yedekleme ler için en iyi uygulamalara][operator-best-practices-storage]bakın.

AKS kümeleri azure disklerle etkileşim edeilgili daha fazla bilgi için [Azure Diskleri için Kubernetes eklentisi'ne][kubernetes-disks]bakın.

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/resource#az-resource-show
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[install-azure-cli]: /cli/azure/install-azure-cli
[azure-files-volume]: azure-files-volume.md
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
