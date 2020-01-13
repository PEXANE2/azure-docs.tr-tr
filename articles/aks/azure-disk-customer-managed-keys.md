---
title: Azure Kubernetes Service (AKS) ' de Azure disklerini şifrelemek için müşteri tarafından yönetilen bir anahtar kullanma
description: AKS işletim sistemini ve veri disklerini şifrelemek için kendi anahtarlarınızı getirin (BYOK).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 01/12/2020
ms.author: mlearned
ms.openlocfilehash: 96e7c401578ca8311bfe0e6b5477a9d8cab1a24e
ms.sourcegitcommit: e9776e6574c0819296f28b43c9647aa749d1f5a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75912722"
---
# <a name="bring-your-own-keys-byok-with-azure-disks-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içindeki Azure diskleriyle kendi anahtarlarınızı (BYOK) getirin

Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, AKS kümeleriniz için hem işletim sistemi hem de veri disklerinin şifrelenmesi için kullanılan [müşteri tarafından yönetilen anahtarlar][customer-managed-keys] sağlayabilirsiniz.

> [!NOTE]
> Linux ve Windows tabanlı AKS kümelerinin her ikisi de desteklenir.

## <a name="before-you-begin"></a>Başlamadan önce

* Bu makalede *Yeni BIR AKS kümesi*oluşturduğunuz varsayılır.  Şifreleme anahtarlarınızı depolamak için bir Azure Key Vault örneği de kullanmanız veya oluşturmanız gerekir.

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

## <a name="create-an-azure-key-vault-instance-to-store-your-keys"></a>Anahtarlarınızı depolamak için bir Azure Key Vault örneği oluşturma

Azure portal, [müşteri tarafından yönetilen anahtarları Azure Key Vault Ile yapılandırmak][byok-azure-portal] için isteğe bağlı olarak kullanabilirsiniz

Yeni bir *kaynak grubu*oluşturun ve ardından yeni bir *Key Vault* örneği oluşturun ve geçici silme ve Temizleme korumasını etkinleştirin.

```azurecli-interactive
# Optionally retrieve Azure region short names for use on upcoming commands
az account list-locations

# Create new resource group in a supported Azure region
az group create -l myAzureRegionName -n myResourceGroup

# Create an Azure Key Vault resource in a supported Azure region
az keyvault create -n myKeyVaultName -g myResourceGroup -l myAzureRegionName  --enable-purge-protection true --enable-soft-delete true
```

## <a name="create-an-instance-of-a-diskencryptionset"></a>DiskEncryptionSet örneği oluşturma

Aşağıdaki adımları tamamlayabilmeniz için Azure Key Vault depolanan bir *anahtara* ihtiyacınız olacak.  Var olan anahtarınızı oluşturduğunuz Key Vault depolayın veya [bir anahtar oluşturun][key-vault-generate]
    
```azurecli-interactive
# Retrieve the Key Vault Id and store it in a variable
keyVaultId=$(az keyvault show --name myKeyVaultName --query [id] -o tsv)

# Retrieve the Key Vault key URL and store it in a variable
keyVaultKeyUrl=$(az keyvault key show --vault-name myKeyVaultName  --name myKeyName  --query [key.kid] -o tsv)

# Create a DiskEncryptionSet
az disk-encryption-set create -n myDiskEncryptionSetName  -l myAzureRegionName  -g myResourceGroup --source-vault $keyVaultId --key-url $keyVaultKeyUrl 
```

## <a name="grant-the-diskencryptionset-resource-access-to-the-key-vault"></a>DiskEncryptionSet kaynağına Anahtar Kasası erişimi verme

Önceki adımlarda oluşturduğunuz DiskEncryptionSet ve Resource gruplarını kullanın ve Azure Key Vault DiskEncryptionSet kaynağına erişimi verin.

```azurecli-interactive
# Retrieve the DiskEncryptionSet value and set a variable
desIdentity=$(az disk-encryption-set show -n myDiskEncryptionSetName  -g myResourceGroup --query [identity.principalId] -o tsv)

# Update security policy settings
az keyvault set-policy -n myKeyVaultName -g myResourceGroup --object-id $desIdentity --key-permissions wrapkey unwrapkey get

# Assign the reader role
az role assignment create --assignee $desIdentity --role Reader --scope $keyVaultId
```

## <a name="create-a-new-aks-cluster-and-encrypt-the-os-disk-with-a-customer-manged-key"></a>Yeni bir AKS kümesi oluşturun ve işletim sistemi diskini, müşteri tarafından yönetilen bir anahtarla şifreleyin

Yeni bir kaynak grubu ve AKS kümesi oluşturun ve ardından anahtarınızı kullanarak işletim sistemi diskini şifreleyin. Müşteri tarafından yönetilen anahtar yalnızca 1,17 'den büyük Kubernetes sürümlerinde desteklenir

```azurecli-interactive
# Retrieve the DiskEncryptionSet value and set a variable
diskEncryptionSetId=$(az resource show -n diskEncryptionSetName -g myResourceGroup --resource-type "Microsoft.Compute/diskEncryptionSets" --query [id] -o tsv)

# Create a resource group for the AKS cluster
az group create -n myResourceGroup-l myAzureRegionName

# Create the AKS cluster
az aks create -n myAKSCluster -g myResourceGroup --node-osdisk-diskencryptionset-id $diskEncryptionSetId --kubernetes-version 1.17.0
```

Yukarıda oluşturulan kümeye yeni düğüm havuzları eklendiğinde, oluşturma sırasında belirtilen müşteri tarafından yönetilen anahtar, işletim sistemi diskini şifrelemek için kullanılır

## <a name="encrypt-your-aks-cluster-data-disk-with-a-customer-managed-key"></a>AKS kümesi veri diskinizi, müşteri tarafından yönetilen bir anahtarla şifreleyin

AKS veri disklerini kendi anahtarınızla de şifreleyebilirsiniz.  MyResourceGroup ve myDiskEncryptionSetName değerlerini gerçek değerlerinizle değiştirin ve YAML 'yi uygulayın.

Uygun AKS kimlik bilgilerine sahip olduğunuzdan emin olun. Hizmet sorumlusunun, diskencryptionset 'in bulunduğu kaynak grubuna katkıda bulunan erişiminin olması gerekir. Aksi takdirde, hizmet sorumlusunun izin içermediğinden emin olmak için bir hata alırsınız.

Aşağıdaki bilgileri içeren **bYok-Azure-disk. YAML** adlı bir dosya oluşturun.  MyResourceGroup ve myDiskEncrptionSetName değerlerini değerlerinizle değiştirin.

```
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: hdd
provisioner: kubernetes.io/azure-disk
parameters:
  skuname: Standard_LRS
  kind: managed
  diskEncryptionSetID: "/subscriptions/{subs-id}/resourceGroups/{myResourceGroup}/providers/Microsoft.Compute/diskEncryptionSets/{myDiskEncryptionSetName}"
```
Ardından, bu dağıtımı AKS kümenizde çalıştırın:
```azurecli-interactive
kubectl apply -f byok-azure-disk.yaml
```

## <a name="limitations"></a>Sınırlamalar

* Kubernetes sürüm 1,17 ve üzerinde işletim sistemi disk şifrelemesi destekleniyor   
* Yalnızca BYOK 'ın desteklendiği bölgelerde kullanılabilir
* Bu şu anda yalnızca yeni AKS kümeleri için mevcut kümeler yükseltilemez
* Sanal makine ölçek kümeleri kullanan AKS kümesi gereklidir, sanal makine kullanılabilirlik kümeleri için destek gerekmez


## <a name="next-steps"></a>Sonraki adımlar

[AKS küme güvenliği için en iyi uygulamaları][best-practices-security] gözden geçirme

<!-- LINKS - external -->

<!-- LINKS - internal -->
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[best-practices-security]: /azure/aks/operator-best-practices-cluster-security
[byok-azure-portal]: /azure/storage/common/storage-encryption-keys-portal
[customer-managed-keys]: /azure/virtual-machines/windows/disk-encryption#customer-managed-keys-public-preview
[key-vault-generate]: /azure/key-vault/key-vault-manage-with-cli2
