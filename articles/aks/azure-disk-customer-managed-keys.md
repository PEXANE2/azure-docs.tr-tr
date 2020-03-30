---
title: Azure Kubernetes Hizmetinde (AKS) Azure disklerini şifrelemek için müşteri tarafından yönetilen bir anahtar kullanın
description: AKS OS ve Veri disklerini şifrelemek için kendi anahtarlarınızı (BYOK) getirin.
services: container-service
ms.topic: article
ms.date: 01/12/2020
ms.openlocfilehash: bb6ba5e6dd4ace9e33043079c0f435c10baf5cb2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596513"
---
# <a name="bring-your-own-keys-byok-with-azure-disks-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Azure diskleriyle kendi anahtarlarınızı (BYOK) getirin

Azure Depolama, bir depolama hesabındaki tüm verileri istirahat halinde şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, AKS kümeleriniz için hem işletim sistemi hem de veri diskleri için şifreleme için kullanmak üzere [müşteri tarafından yönetilen anahtarları][customer-managed-keys] sağlayabilirsiniz.

> [!NOTE]
> BYOK Linux ve Windows tabanlı AKS kümeleri, Azure yönetilen disklerin sunucu tarafı şifrelemesini destekleyen [Azure bölgelerinde][supported-regions] kullanılabilir.

## <a name="before-you-begin"></a>Başlamadan önce

* Bu makalede, yeni bir *AKS kümesi*oluşturduğunuz varsayar.

* Yönetilen diskleri şifrelemek için Key Vault'u kullanırken *Azure Key Vault* için yumuşak silme ve temizleme koruması etkinleştirmelisiniz.

* Azure CLI sürüm 2.0.79 veya sonrası ve aks önizleme 0.4.26 uzantısı gerekir

> [!IMPORTANT]
> AKS önizleme özellikleri self servis tercihidir. Önizlemeler "olduğu gibi" ve "mevcut olduğu gibi" sağlanır ve hizmet düzeyi anlaşmaları ve sınırlı garanti dışında dır. AKS Önizlemeler kısmen en iyi çaba temelinde müşteri desteği tarafından karşılanır. Bu nedenle, bu özellikler üretim kullanımı için değildir. Ek infromation için lütfen aşağıdaki destek makalelerini görmek için:
>
> * [AKS Destek Politikaları](support-policies.md)
> * [Azure Destek SSS](faq.md)

## <a name="install-latest-aks-cli-preview-extension"></a>En son AKS CLI önizleme uzantısını yükleyin

Müşteri tarafından yönetilen anahtarları kullanmak için *aks önizleme* CLI uzantısı sürümü 0.4.26 veya daha yüksek olması gerekir. [az uzantı ekle][az-extension-add] komutunu kullanarak *aks önizleme* Azure CLI uzantısını yükleyin ve az [uzantı güncelleştirme][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="create-an-azure-key-vault-instance"></a>Azure Anahtar Kasası örneği oluşturma

Anahtarlarınızı depolamak için Azure Key Vault örneğini kullanın.  [Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırmak][byok-azure-portal] için Azure portalını isteğe bağlı olarak kullanabilirsiniz

Yeni bir *kaynak grubu*oluşturun, ardından yeni bir Anahtar *Kasa* örneği oluşturun ve yumuşak silme ve temizleme koruması etkinleştirin.  Her komut için aynı bölge ve kaynak grubu adlarını kullandığınızdan emin olun.

```azurecli-interactive
# Optionally retrieve Azure region short names for use on upcoming commands
az account list-locations
```

```azurecli-interactive
# Create new resource group in a supported Azure region
az group create -l myAzureRegionName -n myResourceGroup

# Create an Azure Key Vault resource in a supported Azure region
az keyvault create -n myKeyVaultName -g myResourceGroup -l myAzureRegionName  --enable-purge-protection true --enable-soft-delete true
```

## <a name="create-an-instance-of-a-diskencryptionset"></a>DiskEncryptionSet örneği oluşturma

*MyKeyVaultName'i* anahtar kasanızın adı ile değiştirin.  Ayrıca, aşağıdaki adımları tamamlamak için Azure Key Vault'ta depolanan bir *anahtara* da ihtiyacınız olacaktır.  Mevcut Anahtarınızı önceki adımlarda oluşturduğunuz Key Vault'ta saklayın veya [yeni bir anahtar oluşturun][key-vault-generate] ve aşağıdaki anahtarınızın adı ile *myKeyName'i* değiştirin.
    
```azurecli-interactive
# Retrieve the Key Vault Id and store it in a variable
keyVaultId=$(az keyvault show --name myKeyVaultName --query [id] -o tsv)

# Retrieve the Key Vault key URL and store it in a variable
keyVaultKeyUrl=$(az keyvault key show --vault-name myKeyVaultName  --name myKeyName  --query [key.kid] -o tsv)

# Create a DiskEncryptionSet
az disk-encryption-set create -n myDiskEncryptionSetName  -l myAzureRegionName  -g myResourceGroup --source-vault $keyVaultId --key-url $keyVaultKeyUrl 
```

## <a name="grant-the-diskencryptionset-access-to-key-vault"></a>DiskEncryptionSet'in anahtar kasasına erişimini verme

Önceki adımlarda oluşturduğunuz DiskEncryptionSet'i ve kaynak gruplarını kullanın ve DiskEncryptionSet kaynak erişimini Azure Anahtar Kasası'na ver.

```azurecli-interactive
# Retrieve the DiskEncryptionSet value and set a variable
desIdentity=$(az disk-encryption-set show -n myDiskEncryptionSetName  -g myResourceGroup --query [identity.principalId] -o tsv)

# Update security policy settings
az keyvault set-policy -n myKeyVaultName -g myResourceGroup --object-id $desIdentity --key-permissions wrapkey unwrapkey get

# Assign the reader role
az role assignment create --assignee $desIdentity --role Reader --scope $keyVaultId
```

## <a name="create-a-new-aks-cluster-and-encrypt-the-os-disk"></a>Yeni bir AKS kümesi oluşturun ve işletim sistemi diskini şifreleme

Yeni bir **kaynak grubu** ve AKS kümesi oluşturun ve ardından işletim sistemi diskini şifrelemek için anahtarınızı kullanın. Müşteri tarafından yönetilen anahtarlar yalnızca 1,17'den büyük Kubernetes sürümlerinde desteklenir. 

> [!IMPORTANT]
> AKS kümeniz için yeni bir resoruce grubu oluşturduğunuzdan emin olun

```azurecli-interactive
# Retrieve the DiskEncryptionSet value and set a variable
diskEncryptionSetId=$(az resource show -n mydiskEncryptionSetName -g myResourceGroup --resource-type "Microsoft.Compute/diskEncryptionSets" --query [id] -o tsv)

# Create a resource group for the AKS cluster
az group create -n myResourceGroup -l myAzureRegionName

# Create the AKS cluster
az aks create -n myAKSCluster -g myResourceGroup --node-osdisk-diskencryptionset-id $diskEncryptionSetId --kubernetes-version 1.17.0 --generate-ssh-keys
```

Yukarıda oluşturulan kümeye yeni düğüm havuzları eklendiğinde, oluşturma sırasında sağlanan müşteri tarafından yönetilen anahtar işletim sistemi diskini şifrelemek için kullanılır.

## <a name="encrypt-your-aks-cluster-data-disk"></a>AKS küme veri diskinizi şifreleme

AKS veri disklerini kendi anahtarlarınızla da şifreleyebilirsiniz.

> [!IMPORTANT]
> Uygun AKS kimlik bilgilerine sahip olduğundan emin olun. Hizmet sorumlusunun, diskşifreleme kümesinin dağıtıldığı kaynak grubuna katkıda bulunanlara erişimi olması gerekir. Aksi takdirde, hizmet sorumlusunun izinleri olmadığını düşündüren bir hata alırsınız.

```azurecli-interactive
# Retrieve your Azure Subscription Id from id property as shown below
az account list
```

```
someuser@Azure:~$ az account list
[
  {
    "cloudName": "AzureCloud",
    "id": "666e66d8-1e43-4136-be25-f25bb5de5893",
    "isDefault": true,
    "name": "MyAzureSubscription",
    "state": "Enabled",
    "tenantId": "3ebbdf90-2069-4529-a1ab-7bdcb24df7cd",
    "user": {
      "cloudShellID": true,
      "name": "someuser@azure.com",
      "type": "user"
    }
  }
]
```

Aşağıdaki bilgileri içeren **byok-azure-disk.yaml** adlı bir dosya oluşturun.  myAzureSubscriptionId, myResourceGroup ve myDiskEncrptionSetName'i değerlerinizle değiştirin ve yaml uygulayın.  DiskEncryptionSet'inizin dağıtıldığı kaynak grubunu kullandığınızdan emin olun.  Azure Bulut Kabuğu'nu kullanıyorsanız, bu dosya sanal veya fiziksel bir sistem üzerinde çalışıyormuş gibi vi veya nano kullanılarak oluşturulabilir:

```
kind: StorageClass
apiVersion: storage.k8s.io/v1  
metadata:
  name: hdd
provisioner: kubernetes.io/azure-disk
parameters:
  skuname: Standard_LRS
  kind: managed
  diskEncryptionSetID: "/subscriptions/{myAzureSubscriptionId}/resourceGroups/{myResourceGroup}/providers/Microsoft.Compute/diskEncryptionSets/{myDiskEncryptionSetName}"
```
Ardından, bu dağıtımı AKS kümenizde çalıştırın:
```azurecli-interactive
# Get credentials
az aks get-credentials --name myAksCluster --resource-group myResourceGroup --output table

# Update cluster
kubectl apply -f byok-azure-disk.yaml
```

## <a name="limitations"></a>Sınırlamalar

* BYOK şu anda yalnızca belirli [Azure bölgelerinde][supported-regions] yalnızca GA ve Önizleme'de kullanılabilir
* Kubernetes sürüm 1.17 ve üzeri ile desteklenen Işletim Sistemi Disk Şifrelemesi   
* Yalnızca BYOK'un desteklendiği bölgelerde kullanılabilir
* Şu anda yalnızca yeni AKS kümeleri için müşteri tarafından yönetilen anahtarlarla şifreleme yapılır, varolan kümeler yükseltilemez
* Sanal Makine Ölçek Setleri kullanarak AKS küme gereklidir, Sanal Makine Kullanılabilirlik Setleri için destek


## <a name="next-steps"></a>Sonraki adımlar

[AKS küme güvenliği için en iyi uygulamaları][best-practices-security] gözden geçirin

<!-- LINKS - external -->

<!-- LINKS - internal -->
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[best-practices-security]: /azure/aks/operator-best-practices-cluster-security
[byok-azure-portal]: /azure/storage/common/storage-encryption-keys-portal
[customer-managed-keys]: /azure/virtual-machines/windows/disk-encryption#customer-managed-keys
[key-vault-generate]: /azure/key-vault/key-vault-manage-with-cli2
[supported-regions]: /azure/virtual-machines/windows/disk-encryption#supported-regions
