---
title: Azure Kubernetes Service (AKS) üzerinde Azure diskleri için kapsayıcı depolama arabirimi (CSı) sürücülerini kullanma
description: Azure Kubernetes Service (AKS) kümesinde Azure diskleri için kapsayıcı depolama arabirimi (CSı) sürücülerini nasıl kullanacağınızı öğrenin.
services: container-service
ms.topic: article
ms.date: 08/27/2020
author: palma21
ms.openlocfilehash: 4b5ccd2712a95f5f020daa0161f1b5908a38a62e
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89422087"
---
# <a name="use-the-azure-disk-container-storage-interface-csi-drivers-in-azure-kubernetes-service-aks-preview"></a>Azure Kubernetes Service (AKS) içindeki Azure disk kapsayıcısı depolama arabirimi (CSı) sürücülerini kullanma (Önizleme)
Azure disk CSı sürücüsü, AKS tarafından Azure disklerinin yaşam döngüsünü yönetmek için kullanılan bir [CSI belirtim](https://github.com/container-storage-interface/spec/blob/master/spec.md) uyumlu sürücüsüdür. 

Kapsayıcı depolama arabirimi (CSı), Kubernetes üzerindeki Kapsayıcılı iş yüklerine rastgele blok ve dosya depolama sistemleri sunmak için bir standarttır. Azure Kubernetes hizmeti (AKS), CSı 'yi benimseme ve kullanarak, temel Kubernetes koduna dokunarak ve kendi yayın döngüsünü beklemeden Kubernetes 'te mevcut depolama sistemlerini yeni bir şekilde oluşturabilir, dağıtabilir ve yineleyebilir.

CSı sürücü desteğiyle bir AKS kümesi oluşturmak için bkz. [Azure diskleri ve aks 'de Azure dosyaları IÇIN CSI sürücülerini etkinleştirme](csi-storage-drivers.md).

>[!NOTE]
> *"Ağaç içi Sürücüler"* , temel Kubernetes kodunun parçası olan geçerli depolama sürücülerine ve eklenti olan yeni CSI sürücülerine başvurur.

## <a name="use-csi-persistent-volumes-pv-with-azure-disks"></a>Azure diskleriyle CSı kalıcı birimleri (BD) kullanma 

[Kalıcı bir birim](concepts-storage.md#persistent-volumes) , Kubernetes pods ile kullanılmak üzere sağlanan bir depolama parçasını temsil eder. Kalıcı bir birim bir veya daha fazla sayıda pods tarafından kullanılabilir ve dinamik veya statik olarak sağlanabilir. Bu makalede, Azure Kubernetes Service (AKS) kümesinde tek bir pod tarafından kullanılmak üzere Azure diskleriyle kalıcı birimleri dinamik olarak oluşturma konusu gösterilmektedir. Statik sağlama için bkz. [Azure diskleriyle bir birimi el ile oluşturma ve kullanma](azure-disk-volume.md).

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

Kubernetes birimleri hakkında daha fazla bilgi için bkz. [AKS 'de uygulamalar Için depolama seçenekleri][concepts-storage].

## <a name="dynamically-create-azure-disk-pvs-using-the-built-in-storage-classes"></a>Yerleşik depolama sınıflarını kullanarak dinamik olarak Azure disk PVs oluşturma

Depolama sınıfı, bir depolama biriminin kalıcı bir birimle dinamik olarak nasıl oluşturulduğunu tanımlamak için kullanılır. Kubernetes Depolama sınıfları hakkında daha fazla bilgi için bkz. [Kubernetes Depolama sınıfları][kubernetes-storage-classes]. AKS üzerinde Storage CSı sürücülerini kullanırken, `StorageClasses` **Azure disk CSI depolama sürücülerinden**yararlanan 2 ek yerleşik bulunur. Ek CSı Depolama sınıfları, ağaç içi varsayılan depolama sınıflarının yanı sıra kümeyle oluşturulur.

>[!NOTE]
> *"Ağaç içi Sürücüler"* , eklenti olan çekirdek Kubernetes kodu vs. CSI sürücülerinden bir parçası olan geçerli depolama sürücülerini ifade eder.

- `managed-csi` -Yönetilen bir disk oluşturmak için Azure Standartssd yerel olarak yedekli depolama (LRS) kullanır.
- `managed-csi-premium` -Yönetilen disk oluşturmak için Azure Premium yerel olarak yedekli depolama (LRS) kullanır. 

Her iki depolama sınıflarında de geri kazanma ilkesi, ilgili kalıcı birim silindiğinde temeldeki Azure diskinin silinmesini sağlar. Depolama sınıfları Ayrıca kalıcı birimleri genişletilebilir olacak şekilde yapılandırır, yalnızca yeni boyutla kalıcı birim talebi düzenlemeniz gerekir.

Bu depolama sınıflarından yararlanmak için, yalnızca bir [kalıcı birim talebi (PVC)](concepts-storage.md#persistent-volume-claims) ve bunlara başvuran ve bu öğeleri içeren bir pod oluşturmanız gerekir. Kalıcı bir birim talebi (PVC), depolama sınıfına göre depolamayı otomatik olarak sağlamak için kullanılır. Bir PVC, istenen SKU ve boyut için Azure tarafından yönetilen bir disk oluşturmak üzere önceden oluşturulmuş depolama sınıflarından veya Kullanıcı tanımlı bir depolama sınıfından birini kullanabilir. Pod tanımı oluşturduğunuzda, kalıcı birim talebi istenen depolamayı istemek için belirtilir.

[Kubectl Apply][kubectl-apply] komutuyla örnek bir pod ve ilgili kalıcı birim talebi oluşturun:

```console
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/pvc-azuredisk-csi.yaml
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/nginx-pod-azuredisk.yaml

persistentvolumeclaim/pvc-azuredisk created
pod/nginx-azuredisk created
```

Pod çalışır durumda olduktan sonra adlı yeni bir dosya oluşturun `test.txt` .

```bash
$ kubectl exec nginx-azuredisk -- touch /mnt/azuredisk/test.txt
```

Artık aşağıdaki komutu çalıştırarak diskin doğru şekilde bağlanmış olduğunu ve dosyayı çıktıda gördiğinizi doğrulayarak doğrulayın `test.txt` : 

```console
$ kubectl exec nginx-azuredisk -- ls /mnt/azuredisk

lost+found
outfile
test.txt
```

## <a name="create-a-custom-storage-class"></a>Özel depolama sınıfı oluşturma

Varsayılan depolama sınıfları en yaygın senaryolara göre değil, hepsi değildir. Bazı durumlarda kendi parametrelerinizi kullanarak kendi depolama sınıfınızın özelleştirilmiş olmasını isteyebilirsiniz. ' İ muaf tutmak için, değiştirmek isteyebileceğiniz bir senaryoya sahipsiniz `volumeBindingMode` . 

Varsayılan depolama sınıfları, `volumeBindingMode: Immediate` PersistentVolumeClaim oluşturulduktan hemen sonra meydana gelen garantisini kullanır. Düğüm havuzlarınızın topoloji sınırlı olduğu durumlarda (örneğin Kullanılabilirlik Alanları kullanarak), kalıcı birimler Pod 'un zamanlama gereksinimleri (Bu durumda belirli bir bölgede yer alan) hakkında bilgi sahibi olmadan bağlanır veya temin edilir.

Bu senaryoya yönelik olarak, `volumeBindingMode: WaitForFirstConsumer` PersistentVolumeClaim kullanılarak bir pod olana kadar bir PersistentVolume bağlama ve sağlama işlemini ertelendirip kullanabilirsiniz. Bu şekilde, BD, Pod 'un zamanlama kısıtlamaları tarafından belirtilen kullanılabilirlik bölgesinde (veya başka bir topolojide) uyumlu olacak ve sağlanacak. 

Adlı bir dosya oluşturun `sc-azuredisk-csi-waitforfirstconsumer.yaml` ve aşağıdaki bildirimi yapıştırın.
Depolama sınıfı, `managed-csi` depolama sınıfımız ile aynı ancak farklı `volumeBindingMode` . 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azuredisk-csi-waitforfirstconsumer
provisioner: disk.csi.azure.com
parameters:
  skuname: StandardSSD_LRS 
allowVolumeExpansion: true
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

[Kubectl Apply][kubectl-apply] komutuyla depolama sınıfını oluşturun ve `sc-azuredisk-csi-waitforfirstconsumer.yaml` dosyanızı belirtin:

```console
$ kubectl apply -f sc-azuredisk-csi-waitforfirstconsumer.yaml

storageclass.storage.k8s.io/azuredisk-csi-waitforfirstconsumer created
```

## <a name="volume-snapshots"></a>Birim anlık görüntüleri

Azure disk CSı sürücüsü [kalıcı birimlerin anlık görüntülerini](https://kubernetes-csi.github.io/docs/snapshot-restore-feature.html)oluşturmayı destekler. Bu özelliğin bir parçası olarak, sürücü, parametrede ayarlanan değere (varsayılan olarak true) bağlı olarak *tam* veya [ *artımlı* anlık görüntü](../virtual-machines/windows/disks-incremental-snapshots.md) gerçekleştirebilir `incremental` . 

Tüm parametrelerle ilgili ayrıntılar için bkz. [Volume Snapshot Class Parameters](https://github.com/kubernetes-sigs/azuredisk-csi-driver/blob/master/docs/driver-parameters.md#volumesnapshotclass).

### <a name="create-a-volume-snapshot"></a>Birim anlık görüntüsü oluşturma

Bu özelliği muaf tutmak için, [kubectl Apply][kubectl-apply] komutuyla bir [birim anlık görüntü sınıfı](https://github.com/kubernetes-sigs/azuredisk-csi-driver/blob/master/deploy/example/snapshot/storageclass-azuredisk-snapshot.yaml) oluşturun:

```console
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/snapshot/storageclass-azuredisk-snapshot.yaml

volumesnapshotclass.snapshot.storage.k8s.io/csi-azuredisk-vsc created
```

Şimdi [Bu öğreticinin başlangıcında dinamik olarak oluşturulan](#dynamically-create-azure-disk-pvs-using-the-built-in-storage-classes)PVC 'den bir [birim anlık görüntüsü](https://github.com/kubernetes-sigs/azuredisk-csi-driver/blob/master/deploy/example/snapshot/azuredisk-volume-snapshot.yaml) oluşturalım `pvc-azuredisk` .


```bash
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/snapshot/azuredisk-volume-snapshot.yaml

volumesnapshot.snapshot.storage.k8s.io/azuredisk-volume-snapshot created
```

Anlık görüntünün doğru şekilde oluşturulduğundan emin olun:

```bash
$ kubectl describe volumesnapshot azuredisk-volume-snapshot

Name:         azuredisk-volume-snapshot
Namespace:    default
Labels:       <none>
Annotations:  API Version:  snapshot.storage.k8s.io/v1beta1
Kind:         VolumeSnapshot
Metadata:
  Creation Timestamp:  2020-08-27T05:27:58Z
  Finalizers:
    snapshot.storage.kubernetes.io/volumesnapshot-as-source-protection
    snapshot.storage.kubernetes.io/volumesnapshot-bound-protection
  Generation:        1
  Resource Version:  714582
  Self Link:         /apis/snapshot.storage.k8s.io/v1beta1/namespaces/default/volumesnapshots/azuredisk-volume-snapshot
  UID:               dd953ab5-6c24-42d4-ad4a-f33180e0ef87
Spec:
  Source:
    Persistent Volume Claim Name:  pvc-azuredisk
  Volume Snapshot Class Name:      csi-azuredisk-vsc
Status:
  Bound Volume Snapshot Content Name:  snapcontent-dd953ab5-6c24-42d4-ad4a-f33180e0ef87
  Creation Time:                       2020-08-31T05:27:59Z
  Ready To Use:                        true
  Restore Size:                        10Gi
Events:                                <none>
```

### <a name="create-a-new-pvc-based-on-a-volume-snapshot"></a>Birim anlık görüntüsüne göre yeni bir PVC oluşturma

Birim anlık görüntüsüne göre yeni bir PVC oluşturabilirsiniz. Önceki adımda oluşturulan anlık görüntüyü kullanın ve [Yeni BIR PVC](https://github.com/kubernetes-sigs/azuredisk-csi-driver/blob/master/deploy/example/snapshot/pvc-azuredisk-snapshot-restored.yaml) ve [Yeni Pod](https://github.com/kubernetes-sigs/azuredisk-csi-driver/blob/master/deploy/example/snapshot/nginx-pod-restored-snapshot.yaml) oluşturup bunu tükettin.

```console
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/snapshot/pvc-azuredisk-snapshot-restored.yaml

$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/snapshot/nginx-pod-restored-snapshot.yaml

persistentvolumeclaim/pvc-azuredisk-snapshot-restored created
pod/nginx-restored created
```

Son olarak, içerik denetlenmeden önce aynı PVC 'nin oluşturulmuş olduğundan emin olalım.

```console
$ kubectl exec nginx-restored -- ls /mnt/azuredisk

lost+found
outfile
test.txt
```

Beklenen şekilde, daha önce oluşturmuş olduğunuz dosyayı hala görebiliriz `test.txt` .

## <a name="clone-volumes"></a>Birimleri Kopyala

Kopyalanmış bir birim, var olan bir Kubernetes biriminin yinelemesi olarak tanımlanır. Kubernetes 'te birimleri kopyalama hakkında daha fazla bilgi için, [birim kopyalamaya](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#volume-cloning)yönelik kavramsal belgelere bakın.

Azure diskleri için CSı sürücüsü birim kopyalamayı destekler. Göstermek için, [daha önce oluşturulan](#dynamically-create-azure-disk-pvs-using-the-built-in-storage-classes) ve yeni bir pod 'ın [klonlanan bir birimini](https://github.com/kubernetes-sigs/azuredisk-csi-driver/blob/master/deploy/example/cloning/nginx-pod-restored-cloning.yaml) `azuredisk-pvc` [kullanmak için](https://github.com/kubernetes-sigs/azuredisk-csi-driver/blob/master/deploy/example/cloning/nginx-pod-restored-cloning.yaml)oluşturun.



```console
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/cloning/pvc-azuredisk-cloning.yaml

$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/cloning/nginx-pod-restored-cloning.yaml

persistentvolumeclaim/pvc-azuredisk-cloning created
pod/nginx-restored-cloning created
```

Şimdi, aşağıdaki ' i çalıştırarak kopyalanmış birimin içeriğini denetleyebilir ve oluşturduğumuzu onaylıyoruz `test.txt` .

```console
$ kubectl exec nginx-restored-cloning -- ls /mnt/azuredisk

lost+found
outfile
test.txt
```

## <a name="resize-a-persistent-volume-pv"></a>Kalıcı bir birimi yeniden boyutlandırma (BD)

Bunun yerine, PVC için daha büyük bir birim isteyebilirsiniz. PVC nesnesini düzenleyin ve daha büyük bir boyut belirtin. Bu değişiklik, PersistentVolume 'i yedekleyen temeldeki birimin genişletmesinin tetiklemesini tetikler. 

> [!NOTE] 
> Talebi karşılamak için hiçbir şekilde yeni bir PersistentVolume oluşturulmaz. Bunun yerine, var olan bir birim yeniden boyutlandırılır.

AKS 'de, yerleşik `managed-csi` depolama sınıfı zaten genişlemeye izin verdiğinden, [daha önce bu depolama sınıfıyla oluşturulan PVC 'den](#dynamically-create-azure-disk-pvs-using-the-built-in-storage-classes)yararlanın. PVC bir 10Gi kalıcı birimi istedi, şunu çalıştırarak şunları doğrulayabiliriz:

```console 
$ kubectl exec -it nginx-azuredisk -- df -h /mnt/azuredisk

Filesystem      Size  Used Avail Use% Mounted on
/dev/sdc        9.8G   42M  9.8G   1% /mnt/azuredisk
```
> [!IMPORTANT]
> Şu anda, Azure disk CSı sürücüsü yalnızca, hiçbir düğüm ilişkili olmayan PVC 'leri yeniden boyutlandırmayı destekler (ve birim belirli bir düğüme bağlı değildir).

Bu nedenle, daha önce oluşturulan Pod 'un silinmesine izin verir:

```console
$ kubectl delete -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/nginx-pod-azuredisk.yaml

pod "nginx-azuredisk" deleted
```

Alanı artırarak PVC 'yi genişletelim `spec.resources.requests.storage` :

```console
$ kubectl patch pvc pvc-azuredisk --type merge --patch '{"spec": {"resources": {"requests": {"storage": "15Gi"}}}}'

persistentvolumeclaim/pvc-azuredisk patched
```

Şimdi birimin daha büyük olduğunu doğrulayalim:

```console
$ kubectl get pv

NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                     STORAGECLASS   REASON   AGE
pvc-391ea1a6-0191-4022-b915-c8dc4216174a   15Gi       RWO            Delete           Bound    default/pvc-azuredisk                     managed-csi             2d2h
(...)
```

> [!NOTE] 
> PVC, onunla ilişkili bir pod olana kadar yeni boyutu yansıtmaz.

Yeni bir pod oluşturalım:

```console
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/nginx-pod-azuredisk.yaml

pod/nginx-azuredisk created
```

Son olarak, PVC 'nin boyutunu ve pod 'un içini onaylayın: 
```console
$ kubectl get pvc pvc-azuredisk
NAME            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-azuredisk   Bound    pvc-391ea1a6-0191-4022-b915-c8dc4216174a   15Gi       RWO            managed-csi    2d2h

$ kubectl exec -it nginx-azuredisk -- df -h /mnt/azuredisk
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdc         15G   46M   15G   1% /mnt/azuredisk
```

<!--- ## Shared disk

[Azure shared disks](../virtual-machines/windows/disks-shared.md) is an Azure managed disks feature that enables attaching an Azure disk to agent nodes simultaneously. Attaching a managed disk to multiple agent nodes allows you, for example, to deploy new or migrate existing clustered applications to Azure.

> [!IMPORTANT] Currently, only raw block device (`volumeMode: Block`) is supported by the Azure disk CSI driver. Applications should manage the coordination and control of writes, reads, locks, caches, mounts and fencing on the shared disk which is exposed as raw block device.

Let's create file called `shared-disk.yaml` by copying the below that contains the shared disk storage class and PVC:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: managed-csi-shared
provisioner: disk.csi.azure.com
parameters:
  skuname: Premium_LRS  # Currently shared disk is only available with premium SSD
  maxShares: "2"
  cachingMode: None  # ReadOnly cache is not available for premium SSD with maxShares>1
reclaimPolicy: Delete
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: pvc-azuredisk-shared
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 256Gi  # minimum size of shared disk is 256GB (P15)
  volumeMode: Block
  storageClassName: managed-csi-shared
```

Create the storage class with the [kubectl apply][kubectl-apply] command and specify your `shared-disk.yaml` file:

```console
$ kubectl apply -f shared-disk.yaml

storageclass.storage.k8s.io/managed-csi-shared created
persistentvolumeclaim/pvc-azuredisk-shared created
``` 

Now let's create a file called `deployment-shared.yml` by copying the below:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: deployment-azuredisk
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
      name: deployment-azuredisk
    spec:
      containers:
        - name: deployment-azuredisk
          image: nginx
          volumeDevices:
            - name: azuredisk
              devicePath: /dev/sdx
      volumes:
        - name: azuredisk
          persistentVolumeClaim:
            claimName: pvc-azuredisk-shared
```

Create the deployment with the [kubectl apply][kubectl-apply] command and specify your `deployment-shared.yml` file:

```console
$ kubectl apply -f deployment-shared.yml

deployment/deployment-azuredisk created
```

Finally, let's check the block device inside the pod:

```console
# kubectl exec -it deployment-sharedisk-7454978bc6-xh7jp bash
root@deployment-sharedisk-7454978bc6-xh7jp:/# dd if=/dev/zero of=/dev/sdx bs=1024k count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 0.0502999 s, 2.1 GB/s
```
-->

## <a name="windows-containers"></a>Windows Kapsayıcıları

Azure disk CSı sürücüsü ayrıca Windows düğümlerini ve kapsayıcıları destekler. Windows kapsayıcıları kullanmak istiyorsanız, Windows düğüm havuzu eklemek için [Windows kapsayıcıları öğreticisini](windows-container-cli.md) izleyin.

Windows düğüm havuzunuz olduktan sonra, yalnızca gibi yerleşik depolama sınıflarından yararlanabilirsiniz `managed-csi` . [windows-based stateful set](https://github.com/kubernetes-sigs/azuredisk-csi-driver/blob/master/deploy/example/windows/statefulset.yaml) `data.txt` Aşağıdaki [kubectl Apply][kubectl-apply] komutuyla, zaman damgalarını bir dosyaya kaydeden, örnek Windows tabanlı bir durum bilgisi kümesini dağıtabilirsiniz:

 ```console
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/azuredisk-csi-driver/master/deploy/example/windows/statefulset.yaml

statefulset.apps/busybox-azuredisk created
```

Artık şu çalıştırarak birimin içeriğini doğrulayabilirsiniz:

```console
$ kubectl exec -it busybox-azuredisk-0 -- cat c:\\mnt\\azuredisk\\data.txt # on Linux/MacOS Bash
$ kubectl exec -it busybox-azuredisk-0 -- cat c:\mnt\azuredisk\data.txt # on Windows Powershell/CMD

2020-08-27 08:13:41Z
2020-08-27 08:13:42Z
2020-08-27 08:13:44Z
(...)
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure dosyaları için CSı sürücüsü kullanmayı öğrenmek için bkz. [Azure dosyalarını CSI sürücülerle kullanma](azure-files-csi.md).
- Depolama en iyi uygulamaları hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) içinde depolama ve yedeklemeler Için en iyi uygulamalar][operator-best-practices-storage]


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
[az-extension-add]: /cli/azure/extension?view=azure-cli-latest#az-extension-add
[az-extension-update]: /cli/azure/extension?view=azure-cli-latest#az-extension-update
[az-feature-register]: /cli/azure/feature?view=azure-cli-latest#az-feature-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register