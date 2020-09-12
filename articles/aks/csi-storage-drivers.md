---
title: Azure Kubernetes Service (AKS) üzerinde kapsayıcı depolama arabirimi (CSı) sürücülerini etkinleştirme
description: Azure disk ve Azure dosyaları için Azure Kubernetes hizmeti (AKS) kümesindeki kapsayıcı depolama arabirimi (CSı) sürücülerini nasıl etkinleştireceğinizi öğrenin.
services: container-service
ms.topic: article
ms.date: 08/27/2020
author: palma21
ms.openlocfilehash: bd5706d20496e1ff00843f761443d183cf7fcae3
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89422076"
---
# <a name="enable-container-storage-interface-csi-drivers-for-azure-disks-and-azure-files-on-azure-kubernetes-service-aks-preview"></a>Azure Kubernetes Service (AKS) üzerinde Azure diskleri ve Azure dosyaları için kapsayıcı depolama arabirimi (CSı) sürücülerini etkinleştirme (Önizleme)

Kapsayıcı depolama arabirimi (CSı), Kubernetes üzerindeki Kapsayıcılı iş yüklerine rastgele blok ve dosya depolama sistemleri sunmak için bir standarttır. Azure Kubernetes hizmeti (AKS), CSı 'yi benimseme ve kullanarak, temel Kubernetes koduna dokunarak ve kendi yayın döngüsünü beklemeden Kubernetes 'te mevcut depolama sistemlerini yeni bir şekilde oluşturabilir, dağıtabilir ve yineleyebilir.

AKS üzerinde CSı depolama sürücüsü desteği, yerel olarak yararlanmanızı sağlar:
- [*Azure diskleri*](azure-disk-csi.md) -bir Kubernetes *veri diski* kaynağı oluşturmak için kullanılabilir. Diskler, yüksek performanslı SSD 'Ler veya Azure Standart depolama tarafından desteklenen, normal HDD 'Ler veya standart SSD 'Ler tarafından desteklenen Azure Premium depolama kullanabilir. Çoğu üretim ve geliştirme iş yükleri için Premium depolama kullanın. Azure diskleri *Readwriteonce*olarak bağlanır, bu nedenle yalnızca tek bir pod için kullanılabilir. Aynı anda birden çok dizin tarafından erişilebilen depolama birimlerinde Azure dosyalarını kullanın.
- [*Azure dosyaları*](azure-files-csi.md) , Azure depolama hesabı tarafından desteklenen bir SMB 3,0 paylaşımının pods 'ye bağlanması için kullanılabilir. Dosyalar, verileri birden çok düğümde ve düğüm genelinde paylaşmanızı sağlar. Dosyalar, yüksek performanslı SSD 'Ler tarafından desteklenen normal HDD 'Ler veya Azure Premium Depolama tarafından desteklenen Azure Standart depolama alanını kullanabilir.

> [!IMPORTANT]
> Kubernetes sürüm 1,21 ' den başlayarak Kubernetes yalnızca CSı sürücülerini ve varsayılan olarak kullanır. Bunlar, Kubernetes 'te depolama desteğinin geleceği.
>
> *"Ağaç içi Sürücüler"* , temel Kubernetes kodunun parçası olan geçerli depolama sürücülerine ve eklenti olan yeni CSI sürücülerine başvurur.

## <a name="limitations"></a>Sınırlamalar

- Bu özellik yalnızca küme oluşturma zamanında ayarlanabilir.
- CSı sürücülerini destekleyen en düşük Kubernetes ikincil sürümü v 1.17.
- Önizleme süresince, varsayılan depolama sınıfı hala [ağaç depolama sınıfı ile aynı](concepts-storage.md#storage-classes)olacaktır. Bu özellik genel kullanıma sunulduğunda, varsayılan depolama sınıfı `managed-csi` depolama sınıfı olur ve ağaç içi Depolama sınıfları kaldırılır.
- İlk önizleme aşamasında yalnızca Azure CLı desteklenir.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

### <a name="register-the-enableazurediskfilecsidriver-preview-feature"></a>`EnableAzureDiskFileCSIDriver`Önizleme özelliğini kaydetme

Azure diskleri ve Azure dosyaları için CSı sürücülerinden faydalanabilir bir AKS kümesi oluşturmak için, `EnableAzureDiskFileCSIDriver` aboneliğinizde Özellik bayrağını etkinleştirmeniz gerekir.

`EnableAzureDiskFileCSIDriver`Aşağıdaki örnekte gösterildiği gibi [az Feature Register][az-feature-register] komutunu kullanarak özellik bayrağını kaydedin:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "EnableAzureDiskFileCSIDriver"
```

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu doğrulayın:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableAzureDiskFileCSIDriver')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

### <a name="install-aks-preview-cli-extension"></a>aks-preview CLI uzantısını yükleme

CSı depolama sürücülerini kullanan bir AKS kümesi veya düğüm havuzu oluşturmak için en son *aks-Preview* CLI uzantısına ihtiyacınız vardır. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını veya [az Extension Update][az-extension-update] komutunu kullanarak tüm kullanılabilir güncelleştirmeleri yüklemeyi kullanın:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
``` 


## <a name="create-a-new-cluster-that-can-use-csi-storage-drivers"></a>CSı depolama sürücülerini kullanan yeni bir küme oluşturun

Aşağıdaki CLı komutlarını kullanarak Azure diskleri ve Azure dosyaları için CSı depolama sürücülerinden faydalanabilir yeni bir küme oluşturun. `--aks-custom-headers`Özelliği ayarlamak için bayrağını kullanın `EnableAzureDiskFileCSIDriver` .

Azure Kaynak grubu oluşturun:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location canadacentral
```

CSı depolama sürücüleri desteğiyle AKS kümesini oluşturun.

```azurecli-interactive
# Create an AKS-managed Azure AD cluster
az aks create -g MyResourceGroup -n MyManagedCluster --network-plugin azure -k 1.17.9 --aks-custom-headers EnableAzureDiskFileCSIDriver=true
```

CSı depolama sürücüleri yerine ağaç depolama sürücülerinde kümeler oluşturmak istiyorsanız, özel parametreyi atlayarak bunu yapabilirsiniz `--aks-custom-headers` .


Aşağıdaki çalıştırarak bu düğüme ekleyebileceğiniz Azure disk tabanlı birimlerinin sayısını denetleyin:

```console
$ kubectl get nodes
aks-nodepool1-25371499-vmss000000
aks-nodepool1-25371499-vmss000001
aks-nodepool1-25371499-vmss000002

$ echo $(kubectl get CSINode <NODE NAME> -o jsonpath="{.spec.drivers[1].allocatable.count}")
8
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure diskleri için CSı sürücüsünü kullanın, bkz. [CSI sürücüleriyle Azure disk kullanma](azure-disk-csi.md).
- Azure dosyaları için CSı sürücüsünü kullanın, bkz. [Azure dosyalarını CSI sürücülerle kullanma](azure-files-csi.md).
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