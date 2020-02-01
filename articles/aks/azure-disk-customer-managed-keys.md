---
title: Azure Kubernetes Service (AKS) ' de Azure disklerini şifrelemek için müşteri tarafından yönetilen bir anahtar kullanma
description: AKS işletim sistemini ve veri disklerini şifrelemek için kendi anahtarlarınızı getirin (BYOK).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 01/12/2020
ms.author: mlearned
ms.openlocfilehash: 831a867d1084f7396db0f8aa4c2e2e665eb98732
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76898827"
---
# <a name="bring-your-own-keys-byok-with-azure-disks-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içindeki Azure diskleriyle kendi anahtarlarınızı (BYOK) getirin

Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, AKS kümeleriniz için hem işletim sistemi hem de veri disklerinin şifrelenmesi için kullanılan [müşteri tarafından yönetilen anahtarlar][customer-managed-keys] sağlayabilirsiniz.

> [!NOTE]
> BYOK Linux ve Windows tabanlı AKS kümeleri, Azure yönetilen disklerin sunucu tarafı şifrelemesini destekleyen [Azure bölgelerinde][supported-regions] kullanılabilir.

## <a name="before-you-begin"></a>Başlamadan önce

* Bu makalede *Yeni BIR AKS kümesi*oluşturduğunuz varsayılır.

* Yönetilen diskleri şifrelemek için Key Vault kullanırken *Azure Key Vault* için geçici silme ve Temizleme korumasını etkinleştirmeniz gerekir.

* Azure CLı sürüm 2.0.79 veya üzeri ve aks-Preview 0.4.26 uzantısı gerekir

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis kabul etme sürecindedir. Önizlemeler, "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi sözleşmelerinden ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin en iyi çaba temelinde kısmen ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri](support-policies.md)
> * [Azure desteği SSS](faq.md)

## <a name="install-latest-aks-cli-preview-extension"></a>En son AKS CLı önizleme uzantısını yükler

Müşteri tarafından yönetilen anahtarları kullanmak için, *aks-Preview* CLI uzantısının sürüm 0.4.26 veya daha yüksek olması gerekir. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükledikten sonra [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="create-an-azure-key-vault-instance"></a>Azure Key Vault örneği oluşturma

Anahtarlarınızı depolamak için bir Azure Key Vault örneği kullanın.  Azure portal, [müşteri tarafından yönetilen anahtarları Azure Key Vault Ile yapılandırmak][byok-azure-portal] için isteğe bağlı olarak kullanabilirsiniz

Yeni bir *kaynak grubu*oluşturun ve ardından yeni bir *Key Vault* örneği oluşturun ve geçici silme ve Temizleme korumasını etkinleştirin.  Her komut için aynı bölgeyi ve kaynak grubu adlarını kullandığınızdan emin olun.

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

*Mykeyvaultname* değerini anahtar kasanızın adıyla değiştirin.  Ayrıca, aşağıdaki adımları tamamlayabilmeniz için Azure Key Vault depolanan bir *anahtara* ihtiyacınız olacaktır.  Mevcut anahtarınızı önceki adımlarda oluşturduğunuz Key Vault depolayın veya [Yeni bir anahtar oluşturun][key-vault-generate] ve aşağıdaki *mykeyname* değerini anahtarınızın adıyla değiştirin.
    
```azurecli-interactive
# Retrieve the Key Vault Id and store it in a variable
keyVaultId=$(az keyvault show --name myKeyVaultName --query [id] -o tsv)

# Retrieve the Key Vault key URL and store it in a variable
keyVaultKeyUrl=$(az keyvault key show --vault-name myKeyVaultName  --name myKeyName  --query [key.kid] -o tsv)

# Create a DiskEncryptionSet
az disk-encryption-set create -n myDiskEncryptionSetName  -l myAzureRegionName  -g myResourceGroup --source-vault $keyVaultId --key-url $keyVaultKeyUrl 
```

## <a name="grant-the-diskencryptionset-access-to-key-vault"></a>DiskEncryptionSet erişimini Anahtar Kasası 'na verme

Önceki adımlarda oluşturduğunuz DiskEncryptionSet ve Resource gruplarını kullanın ve Azure Key Vault DiskEncryptionSet kaynağına erişimi verin.

```azurecli-interactive
# Retrieve the DiskEncryptionSet value and set a variable
desIdentity=$(az disk-encryption-set show -n myDiskEncryptionSetName  -g myResourceGroup --query [identity.principalId] -o tsv)

# Update security policy settings
az keyvault set-policy -n myKeyVaultName -g myResourceGroup --object-id $desIdentity --key-permissions wrapkey unwrapkey get

# Assign the reader role
az role assignment create --assignee $desIdentity --role Reader --scope $keyVaultId
```

## <a name="create-a-new-aks-cluster-and-encrypt-the-os-disk"></a>Yeni bir AKS kümesi oluşturma ve işletim sistemi diskini şifreleme

Yeni bir **kaynak grubu** ve aks kümesi oluşturun ve ardından anahtarınızı kullanarak işletim sistemi diskini şifreleyin. Müşteri tarafından yönetilen anahtarlar yalnızca 1,17 'den büyük Kubernetes sürümlerinde desteklenir. 

> [!IMPORTANT]
> Aks kümeniz için yeni bir eklendi grubu oluşturduğunuzdan emin olun

```azurecli-interactive
# Retrieve the DiskEncryptionSet value and set a variable
diskEncryptionSetId=$(az resource show -n mydiskEncryptionSetName -g myResourceGroup --resource-type "Microsoft.Compute/diskEncryptionSets" --query [id] -o tsv)

# Create a resource group for the AKS cluster
az group create -n myResourceGroup -l myAzureRegionName

# Create the AKS cluster
az aks create -n myAKSCluster -g myResourceGroup --node-osdisk-diskencryptionset-id $diskEncryptionSetId --kubernetes-version 1.17.0 --generate-ssh-keys
```

Yukarıda oluşturulan kümeye yeni düğüm havuzları eklendiğinde, oluşturma sırasında belirtilen müşteri tarafından yönetilen anahtar, işletim sistemi diskini şifrelemek için kullanılır.

## <a name="encrypt-your-aks-cluster-data-disk"></a>AKS kümesi veri diskinizi şifreleyin

AKS veri disklerini kendi anahtarınızla de şifreleyebilirsiniz.

> [!IMPORTANT]
> Uygun AKS kimlik bilgilerine sahip olduğunuzdan emin olun. Hizmet sorumlusunun, diskencryptionset 'in dağıtıldığı kaynak grubuna katkıda bulunan erişiminin olması gerekir. Aksi takdirde, hizmet sorumlusunun izin içermediğinden emin olmak için bir hata alırsınız.

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

Aşağıdaki bilgileri içeren **bYok-Azure-disk. YAML** adlı bir dosya oluşturun.  Myazuyeniden gönderme Scriptionıd, myResourceGroup ve myDiskEncrptionSetName değerlerini değerlerinizle değiştirin ve YAML 'yi uygulayın.  DiskEncryptionSet dosyanızın dağıtıldığı kaynak grubunu kullandığınızdan emin olun.  Azure Cloud Shell kullanırsanız, bu dosya bir sanal veya fiziksel sistemde çalışırken VI veya nano kullanılarak oluşturulabilir:

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

* BYOK, yalnızca belirli [Azure BÖLGELERINDE][supported-regions] GA ve önizleme sürümünde kullanılabilir
* Kubernetes sürüm 1,17 ve üzerinde işletim sistemi disk şifrelemesi destekleniyor   
* Yalnızca BYOK 'ın desteklendiği bölgelerde kullanılabilir
* Müşteri tarafından yönetilen anahtarlarla şifreleme Şu anda yalnızca yeni AKS kümelerine yöneliktir, mevcut kümeler yükseltilemez
* Sanal makine ölçek kümeleri kullanan AKS kümesi gereklidir, sanal makine kullanılabilirlik kümeleri için destek gerekmez


## <a name="next-steps"></a>Sonraki adımlar

[AKS küme güvenliği için en iyi uygulamaları][best-practices-security] gözden geçirme

<!-- LINKS - external -->

<!-- LINKS - internal -->
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[best-practices-security]: /azure/aks/operator-best-practices-cluster-security
[byok-azure-portal]: /azure/storage/common/storage-encryption-keys-portal
[customer-managed-keys]: /azure/virtual-machines/windows/disk-encryption#customer-managed-keys
[key-vault-generate]: /azure/key-vault/key-vault-manage-with-cli2
[supported-regions]: /azure/virtual-machines/windows/disk-encryption#supported-regions
