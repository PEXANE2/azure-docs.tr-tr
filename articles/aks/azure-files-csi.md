---
title: Azure Kubernetes Service (AKS) üzerinde Azure dosyaları için kapsayıcı depolama arabirimi (CSı) sürücülerini kullanma
description: Azure Kubernetes Service (AKS) kümesinde Azure dosyaları için kapsayıcı depolama arabirimi (CSı) sürücülerini nasıl kullanacağınızı öğrenin.
services: container-service
ms.topic: article
ms.date: 08/27/2020
author: palma21
ms.openlocfilehash: 018275b6db4c2d2d1059f35077f74a6f45ec3ba9
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89422086"
---
# <a name="use-the-azure-files-container-storage-interface-csi-drivers-in-azure-kubernetes-service-aks-preview"></a>Azure Kubernetes Service (AKS) içindeki Azure dosya kapsayıcısı depolama arabirimi (CSı) sürücülerini kullanma (Önizleme)
Azure Files CSı sürücüsü, Azure dosya paylaşımlarının yaşam döngüsünü yönetmek için AKS tarafından kullanılan bir [CSI belirtimiyle](https://github.com/container-storage-interface/spec/blob/master/spec.md) uyumlu bir sürücü. 

Kapsayıcı depolama arabirimi (CSı), Kubernetes üzerindeki Kapsayıcılı iş yüklerine rastgele blok ve dosya depolama sistemleri sunmak için bir standarttır. Azure Kubernetes hizmeti (AKS), CSı 'yi benimseme ve kullanma yoluyla, temel Kubernetes koduna dokunarak ve kendi yayın döngüsünü beklemeden Kubernetes 'te var olan depolama sistemlerini yeni bir şekilde oluşturabilir, dağıtabilir ve yineleyebilirsiniz.

CSı sürücü desteğiyle bir AKS kümesi oluşturmak için bkz. [Azure diskleri ve aks 'de Azure dosyaları IÇIN CSI sürücülerini etkinleştirme](csi-storage-drivers.md).

>[!NOTE]
> *"Ağaç içi Sürücüler"* , temel Kubernetes kodunun parçası olan geçerli depolama sürücülerine ve eklenti olan yeni CSI sürücülerine başvurur.

## <a name="use-a-persistent-volume-pv-with-azure-files"></a>Azure dosyaları ile kalıcı bir birim (BD) kullanma

[Kalıcı bir birim](concepts-storage.md#persistent-volumes) , Kubernetes pods ile kullanılmak üzere sağlanan bir depolama parçasını temsil eder. Kalıcı bir birim bir veya daha fazla sayıda pods tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Aynı depolama birimine eşzamanlı olarak birden çok Pod erişimi gerekiyorsa, [sunucu ileti bloğu (SMB) protokolünü][smb-overview]kullanarak bağlanmak için Azure dosyalarını kullanabilirsiniz. Bu makalede, bir Azure Kubernetes Service (AKS) kümesinde birden çok düğüm tarafından kullanılmak üzere Azure dosya paylaşımının nasıl dinamik olarak oluşturulacağı gösterilmektedir. Statik sağlama için bkz. [Azure dosya paylaşımıyla bir birimi el ile oluşturma ve kullanma](azure-files-volume.md).

Kubernetes birimleri hakkında daha fazla bilgi için bkz. [AKS 'de uygulamalar Için depolama seçenekleri][concepts-storage].

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="dynamically-create-azure-files-pvs-using-the-built-in-storage-classes"></a>Yerleşik depolama sınıflarını kullanarak Azure dosyaları PVs 'yi dinamik olarak oluşturma
Bir Azure dosya paylaşımının nasıl oluşturulduğunu tanımlamak için bir depolama sınıfı kullanılır. Depolama hesabı, Azure dosya paylaşımlarını tutmak üzere depolama sınıfıyla kullanılmak üzere [düğüm kaynak grubunda][node-resource-group] otomatik olarak oluşturulur. *Skuname*Için aşağıdaki [Azure depolama yedekliliği][storage-skus] arasından seçim yapın:

* *Standard_LRS* -standart yerel olarak yedekli depolama
* *Standard_GRS* -standart coğrafi olarak yedekli depolama
* *Standard_ZRS* -standart bölge yedekli depolama
* *Standard_RAGRS* -standart Okuma Erişimli Coğrafi olarak yedekli depolama
* *Premium_LRS* -Premium yerel olarak yedekli depolama

> [!NOTE]
> Azure dosyaları Premium depolamayı destekler, en düşük Premium dosya paylaşma 100 GB 'dir.

AKS üzerinde Storage CSı sürücülerini kullanırken, `StorageClasses` **Azure dosyaları CSI depolama sürücülerinden**yararlanan 2 ek yerleşik bulunur. Ek CSı Depolama sınıfları, ağaç içi varsayılan depolama sınıflarının yanı sıra kümeyle oluşturulur.

- `azurefile-csi` -Azure Standart depolama kullanarak bir Azure dosya paylaşımının oluşturulmasını sağlar. 
- `azurefile-csi-premium` -Azure dosya paylaşımının oluşturulması için Azure Premium depolama kullanır. 

Her iki depolama sınıflarında de geri kazanma ilkesi, ilgili kalıcı birim silindiğinde temeldeki Azure dosya paylaşımının silinmesini sağlar. Depolama sınıfları Ayrıca, dosya paylaşımlarını genişletilebilir olacak şekilde yapılandırır, yalnızca yeni boyutla kalıcı birim talebi düzenlemeniz gerekir.

Bu depolama sınıflarından yararlanmak için, kalıcı bir [birim talebi (PVC)](concepts-storage.md#persistent-volume-claims) ve bunlara başvuran ve bunlara karşılık gelen bir pod oluşturun. Kalıcı bir birim talebi (PVC), depolama sınıfına göre depolamayı otomatik olarak sağlamak için kullanılır. Bir PVC, istenen SKU ve boyut için bir Azure dosya paylaşımının oluşturulması için önceden oluşturulmuş depolama sınıflarından birini veya Kullanıcı tanımlı bir depolama sınıfını kullanabilir. Pod tanımı oluşturduğunuzda, kalıcı birim talebi istenen depolamayı istemek için belirtilir.

Şu anki tarihi, [kubectl Apply][kubectl-apply] komutuyla birlikte içeren [bir `outfile` kalıcı birim talebi ve pod](https://github.com/kubernetes-sigs/azurefile-csi-driver/blob/master/deploy/example/statefulset.yaml) oluşturun:

```console
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azurefile-csi-driver/master/deploy/example/pvc-azurefile-csi.yaml
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azurefile-csi-driver/master/deploy/example/nginx-pod-azurefile.yaml

persistentvolumeclaim/pvc-azurefile created
pod/nginx-azurefile created
```

Pod çalışır durumda olduktan sonra, aşağıdaki komutu çalıştırarak ve çıktının şunu içerdiğini doğrulayarak dosya paylaşımının doğru şekilde takıldığını doğrulayabilirsiniz `outfile` : 

```console
$ kubectl exec nginx-azurefile -- ls -l /mnt/azurefile

total 29
-rwxrwxrwx 1 root root 29348 Aug 31 21:59 outfile
```

## <a name="create-a-custom-storage-class"></a>Özel depolama sınıfı oluşturma

Varsayılan depolama sınıfları en yaygın senaryolara göre değil, hepsi değildir. Bazı durumlarda kendi parametrelerinizi kullanarak kendi depolama sınıfınızın özelleştirilmiş olmasını isteyebilirsiniz. Örneğin, dosya paylaşımının yapılandırmasını yapılandırmak için aşağıdaki bildirimi kullanın `mountOptions` .

*FileMode* ve *dirmode* Için varsayılan değer, Kubernetes bağlı dosya paylaşımları için *0777* ' dir. Depolama sınıfı nesnesinde farklı bağlama seçeneklerini belirtebilirsiniz.

Adlı bir dosya oluşturun `azure-file-sc.yaml` ve aşağıdaki örnek bildirimi yapıştırın: 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: my-azurefile
provisioner: file.csi.azure.com
reclaimPolicy: Delete
volumeBindingMode: Immediate
allowVolumeExpansion: true
mountOptions:
  - dir_mode=0640
  - file_mode=0640
  - uid=0
  - gid=0
  - mfsymlinks
  - cache=strict # https://linux.die.net/man/8/mount.cifs
  - nosharesock
parameters:
  skuName: Standard_LRS
```

[Kubectl Apply][kubectl-apply] komutuyla depolama sınıfını oluşturun:

```console
kubectl apply -f azure-file-sc.yaml

storageclass.storage.k8s.io/my-azurefile created
```

Azure Files CSı sürücüsü, kalıcı birimlerin ve temel dosya paylaşımlarının [anlık görüntülerini](https://kubernetes-csi.github.io/docs/snapshot-restore-feature.html) oluşturmayı destekler. 

[Kubectl Apply][kubectl-apply] komutuyla bir [birim anlık görüntü sınıfı](https://github.com/kubernetes-sigs/azurefile-csi-driver/blob/master/deploy/example/snapshot/volumesnapshotclass-azurefile.yaml) oluşturun:

```console
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azurefile-csi-driver/master/deploy/example/snapshot/volumesnapshotclass-azurefile.yaml

volumesnapshotclass.snapshot.storage.k8s.io/csi-azurefile-vsc created
```

[Bu öğreticinin başlangıcında dinamik olarak oluşturulan](#dynamically-create-azure-files-pvs-using-the-built-in-storage-classes)PVC 'den bir [birim anlık görüntüsü](https://github.com/kubernetes-sigs/azurefile-csi-driver/blob/master/deploy/example/snapshot/volumesnapshot-azurefile.yaml) oluşturun `pvc-azurefile` .


```bash
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azurefile-csi-driver/master/deploy/example/snapshot/volumesnapshot-azurefile.yaml


volumesnapshot.snapshot.storage.k8s.io/azurefile-volume-snapshot created
```

Anlık görüntünün doğru şekilde oluşturulduğunu doğrulayın:

```bash
$ kubectl describe volumesnapshot azurefile-volume-snapshot

Name:         azurefile-volume-snapshot
Namespace:    default
Labels:       <none>
Annotations:  API Version:  snapshot.storage.k8s.io/v1beta1
Kind:         VolumeSnapshot
Metadata:
  Creation Timestamp:  2020-08-27T22:37:41Z
  Finalizers:
    snapshot.storage.kubernetes.io/volumesnapshot-as-source-protection
    snapshot.storage.kubernetes.io/volumesnapshot-bound-protection
  Generation:        1
  Resource Version:  955091
  Self Link:         /apis/snapshot.storage.k8s.io/v1beta1/namespaces/default/volumesnapshots/azurefile-volume-snapshot
  UID:               c359a38f-35c1-4fb1-9da9-2c06d35ca0f4
Spec:
  Source:
    Persistent Volume Claim Name:  pvc-azurefile
  Volume Snapshot Class Name:      csi-azurefile-vsc
Status:
  Bound Volume Snapshot Content Name:  snapcontent-c359a38f-35c1-4fb1-9da9-2c06d35ca0f4
  Ready To Use:                        false
Events:                                <none>
```

## <a name="resize-a-persistent-volume-pv"></a>Kalıcı bir birimi yeniden boyutlandırma (BD)

Bir PVC için daha büyük bir birim isteyebilirsiniz. PVC nesnesini düzenleyin ve daha büyük bir boyut belirtin. Bu değişiklik, PersistentVolume 'i yedekleyen temeldeki birimin genişletmesinin tetiklemesini tetikler. 

> [!NOTE] 
> Talebi karşılamak için hiçbir şekilde yeni bir PersistentVolume oluşturulmaz. Bunun yerine, var olan bir birim yeniden boyutlandırılır.

AKS 'de, yerleşik `azurefile-csi` depolama sınıfı genişletmeyi zaten desteklediğinden, [Bu depolama sınıfıyla daha önce oluşturulmuş olan PVC 'den](#dynamically-create-azure-files-pvs-using-the-built-in-storage-classes)yararlanın. PVC bir 100Gi dosya paylaşma istedi, şunu çalıştırarak şunları doğrulayabiliriz:

```console 
$ kubectl exec -it nginx-azurefile -- df -h /mnt/azurefile

Filesystem                                                                                Size  Used Avail Use% Mounted on
//f149b5a219bd34caeb07de9.file.core.windows.net/pvc-5e5d9980-da38-492b-8581-17e3cad01770  100G  128K  100G   1% /mnt/azurefile
```

Alanı artırarak PVC 'yi genişletin `spec.resources.requests.storage` :

```console
$ kubectl patch pvc pvc-azurefile --type merge --patch '{"spec": {"resources": {"requests": {"storage": "200Gi"}}}}'

persistentvolumeclaim/pvc-azurefile patched
```

Pod içindeki hem PVC 'nin hem de FileSystem 'ın yeni boyutu göstermesini doğrulayın:

```console
$ kubectl get pvc pvc-azurefile
NAME            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS    AGE
pvc-azurefile   Bound    pvc-5e5d9980-da38-492b-8581-17e3cad01770   200Gi      RWX            azurefile-csi   64m

$ kubectl exec -it nginx-azurefile -- df -h /mnt/azurefile
Filesystem                                                                                Size  Used Avail Use% Mounted on
//f149b5a219bd34caeb07de9.file.core.windows.net/pvc-5e5d9980-da38-492b-8581-17e3cad01770  200G  128K  200G   1% /mnt/azurefile
```

## <a name="windows-containers"></a>Windows Kapsayıcıları

Windows kapsayıcıları kullanmak istiyorsanız, Azure Files CSı sürücüsü de Windows düğümleri ve kapsayıcıları destekler, Windows kapsayıcıları [öğreticisini](windows-container-cli.md) kullanarak bir Windows düğüm havuzu ekleyin.

Windows düğüm havuzunuz olduktan sonra, gibi yerleşik depolama sınıflarından yararlanın `azurefile-csi` veya özel bir tane oluşturun. [windows-based stateful set](https://github.com/kubernetes-sigs/azurefile-csi-driver/blob/master/deploy/example/windows/statefulset.yaml) `data.txt` Aşağıdaki [kubectl Apply][kubectl-apply] komutuyla, zaman damgalarını bir dosyaya kaydeden, örnek Windows tabanlı bir durum bilgisi kümesini dağıtabilirsiniz:

 ```console
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azurefile-csi-driver/master/deploy/example/windows/statefulset.yaml

statefulset.apps/busybox-azuredisk created
```

Şu çalıştırarak birimin içeriğini doğrulayın:

```console
$ kubectl exec -it busybox-azurefile-0 -- cat c:\\mnt\\azurefile\\data.txt # on Linux/MacOS Bash
$ kubectl exec -it busybox-azurefile-0 -- cat c:\mnt\azurefile\data.txt # on Windows Powershell/CMD

2020-08-27 22:11:01Z
2020-08-27 22:11:02Z
2020-08-27 22:11:04Z
(...)
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure diskleri için CSı sürücüsünü nasıl kullanacağınızı öğrenmek için bkz. [CSI sürücüleriyle Azure disklerini kullanma](azure-disk-csi.md).
- Depolama en iyi uygulamaları hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) içinde depolama ve yedeklemeler Için en iyi uygulamalar][operator-best-practices-storage]


<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview


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
[az-extension-add]: /cli/azure/extension?view=azure-cli-latest#az-extension-add
[az-extension-update]: /cli/azure/extension?view=azure-cli-latest#az-extension-update
[az-feature-register]: /cli/azure/feature?view=azure-cli-latest#az-feature-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[node-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[storage-skus]: ../storage/common/storage-redundancy.md