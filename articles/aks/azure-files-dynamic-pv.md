---
title: Azure Dosyaları paylaşımını dinamik olarak oluşturun
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmetinde (AKS) birden çok eşzamanlı bölmeyle kullanılmak üzere Azure Dosyaları ile kalıcı bir birim oluşturmayı öğrenin
services: container-service
ms.topic: article
ms.date: 09/12/2019
ms.openlocfilehash: 59b773cd4608187fedb24358eac57715e1c271ea
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80803543"
---
# <a name="dynamically-create-and-use-a-persistent-volume-with-azure-files-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Azure Dosyaları ile kalıcı bir birim oluşturun ve kullanın

Kalıcı bir birim, Kubernetes bölmelerinde kullanılmak üzere sağlanmış bir depolama parçasını temsil eder. Kalıcı bir birim bir veya çok bölme tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Birden çok bölmenin aynı depolama birimine eşzamanlı olarak erişmeye ihtiyacı varsa, [Sunucu İleti Bloğu (SMB) protokolünü][smb-overview]kullanarak bağlanmak için Azure Dosyaları'nı kullanabilirsiniz. Bu makalede, bir Azure Kubernetes Hizmeti (AKS) kümesinde birden çok bölme tarafından kullanılmak üzere bir Azure Dosyaları paylaşımını dinamik olarak nasıl oluşturabileceğinizgösterilmektedir.

Kubernetes ciltleri hakkında daha fazla bilgi için [AKS'deki uygulamalar için Depolama seçeneklerine][concepts-storage]bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="create-a-storage-class"></a>Depolama sınıfı oluşturma

Bir depolama sınıfı, Azure dosya paylaşımının nasıl oluşturulduğunu tanımlamak için kullanılır. Azure dosya paylaşımlarını tutmak için depolama sınıfıyla kullanılmak üzere [düğüm kaynak grubunda][node-resource-group] otomatik olarak bir depolama hesabı oluşturulur. *skuName*için aşağıdaki [Azure depolama artıklığını][storage-skus] seçin:

* *Standard_LRS* - standart yerel yedekli depolama (LRS)
* *Standard_GRS* - standart jeo-yedekli depolama (GRS)
* *Standard_ZRS* - standart bölge yedekli depolama (ZRS)
* *Standard_RAGRS* - standart okuma-erişim coğrafi yedekli depolama (RA-GRS)
* *Premium_LRS* - premium yerel yedekdepolama (LRS)
* *Premium_ZRS* - premium bölge yedekli depolama (GRS)

> [!NOTE]
> Azure Dosyaları, Kubernetes 1,13 veya daha yüksek çalıştıran AKS kümelerinde premium depolamayı destekler, minimum premium dosya paylaşımı 100 GB'dır

Azure Dosyaları için Kubernetes depolama sınıfları hakkında daha fazla bilgi için [Kubernetes Depolama Sınıfları'na][kubernetes-storage-classes]bakın.

Adlandırılmış `azure-file-sc.yaml` bir dosya oluşturun ve aşağıdaki örnek bildirimde kopyalayın. *MountOptions*hakkında daha fazla bilgi için [Mount seçenekleri][mount-options] bölümüne bakın.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=0
  - gid=0
  - mfsymlinks
  - cache=strict
parameters:
  skuName: Standard_LRS
```

[Kubectl uygula][kubectl-apply] komutu ile depolama sınıfını oluşturun:

```console
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-a-persistent-volume-claim"></a>Kalıcı bir birim talebi oluşturma

Kalıcı bir birim talebi (PVC), Azure dosya paylaşımını dinamik olarak sağlamak için depolama sınıfı nesnesini kullanır. Aşağıdaki YAML *ReadWriteMany* erişimi ile 5 *GB* boyutunda kalıcı bir hacim talebi oluşturmak için kullanılabilir. Erişim modları hakkında daha fazla bilgi için [Kubernetes kalıcı birim][access-modes] belgelerine bakın.

Şimdi adlı `azure-file-pvc.yaml` bir dosya oluşturun ve aşağıdaki YAML kopyalayın. *StorageClassName'nin* son adımda oluşturulan depolama sınıfıyla eşleştiğinden emin olun:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

> [!NOTE]
> Depolama sınıfınız için *Premium_LRS* sku kullanıyorsanız, *depolama* için minimum değer *100Gi*olmalıdır.

[Kubectl uygula][kubectl-apply] komutu ile kalıcı hacim iddiasını oluşturun:

```console
kubectl apply -f azure-file-pvc.yaml
```

Tamamlandıktan sonra, dosya paylaşımı oluşturulur. Bağlantı bilgileri ve kimlik bilgilerini içeren bir Kubernetes sırrı da oluşturulur. PVC durumunu görüntülemek için [kubectl get][kubectl-get] komutunu kullanabilirsiniz:

```console
$ kubectl get pvc azurefile

NAME        STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
azurefile   Bound     pvc-8436e62e-a0d9-11e5-8521-5a8664dc0477   5Gi        RWX            azurefile      5m
```

## <a name="use-the-persistent-volume"></a>Kalıcı birimi kullanma

Aşağıdaki YAML, */mnt/azure* yoluna Azure dosya paylaşımını monte etmek için kalıcı birim talebi *azuredosyasını* kullanan bir bölme oluşturur. Windows Server kapsayıcıları için (şu anda AKS'de önizlemede), *'D:'* gibi Windows yolu kuralını kullanan bir *mountPath* belirtin.

Adlı `azure-pvc-files.yaml`bir dosya oluşturun ve aşağıdaki YAML'de kopyalayın. *ClaimName'nin* son adımda oluşturulan PVC ile eşleştiğinden emin olun.

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
        claimName: azurefile
```

[Kubectl uygula][kubectl-apply] komutu ile pod oluşturun.

```console
kubectl apply -f azure-pvc-files.yaml
```

Artık */mnt/azure* dizininde yer alan Azure Dosyaları paylaşımınızla çalışan bir bölmeniz var. Bu yapılandırma, pod'unuzu incelerken `kubectl describe pod mypod`görülebilir. Aşağıdaki yoğunlaştırılmış örnek çıktı, kapsayıcıya monte edilen hacmi gösterir:

```
Containers:
  mypod:
    Container ID:   docker://053bc9c0df72232d755aa040bfba8b533fa696b123876108dec400e364d2523e
    Image:          nginx:1.15.5
    Image ID:       docker-pullable://nginx@sha256:d85914d547a6c92faa39ce7058bd7529baacab7e0cd4255442b04577c4d1f424
    State:          Running
      Started:      Fri, 01 Mar 2019 23:56:16 +0000
    Ready:          True
    Mounts:
      /mnt/azure from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8rv4z (ro)
[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azurefile
    ReadOnly:   false
[...]
```

## <a name="mount-options"></a>Bağlama seçenekleri

*fileMode* ve *dirMode* için varsayılan değer Kubernetes sürüm 1.13.0 ve üzeri için *0777'dir.* Depolama sınıfıyla kalıcı birim dinamik olarak oluşturuyorsa, depolama sınıfı nesnesi üzerinde montaj seçenekleri belirtilebilir. Aşağıdaki örnek *0777*ayarlar:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=0
  - gid=0
  - mfsymlinks
  - cache=strict
parameters:
  skuName: Standard_LRS
```

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi uygulamalar [için, AKS'deki depolama ve yedekleme ler için en iyi uygulamalara][operator-best-practices-storage]bakın.

Azure Dosyaları'nı kullanarak Kubernetes kalıcı birimleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure Dosyaları için Kubernetes eklentisi][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/aks#az-aks-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[storage-skus]: ../storage/common/storage-redundancy.md
[kubernetes-rbac]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
[node-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
