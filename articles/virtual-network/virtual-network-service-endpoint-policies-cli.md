---
title: Veri sızmasını Azure Depolama ile sınırlandırın - Azure CLI
description: Bu makalede, Azure CLI'yi kullanarak sanal ağ hizmeti bitiş noktası ilkeleriyle sanal ağ veri filtrelemeyi Azure Depolama kaynaklarıyla nasıl sınırlandırabileceğinizi ve sınırlandırabileceğinizi öğreneceksiniz.
services: virtual-network
documentationcenter: virtual-network
author: rdhillon
manager: ''
editor: ''
tags: azure-resource-manager
Customer intent: I want only specific Azure Storage account to be allowed access from a virtual network subnet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: rdhillon
ms.custom: ''
ms.openlocfilehash: e01af052a936403162115965f2dc5b3ad46dd9cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78271176"
---
# <a name="manage-data-exfiltration-to-azure-storage-accounts-with-virtual-network-service-endpoint-policies-using-the-azure-cli"></a>Azure CLI'yi kullanarak sanal ağ hizmeti bitiş noktası ilkeleriyle Azure Depolama hesaplarına veri sızma larını yönetme

Sanal ağ hizmeti bitiş noktası ilkeleri, hizmet bitiş noktaları üzerinden sanal ağ içinden Azure Depolama hesaplarına erişim denetimi uygulamanızı sağlar. Bu, iş yüklerinizi güvence altına almanın, hangi depolama hesaplarına izin verildiğini ve veri sızmasına izin verilen yerlerde yöneticilik için bir anahtardır.
Bu makalede şunları öğreneceksiniz:

* Sanal ağ oluşturun ve bir alt ağ ekleyin.
* Azure Depolama için hizmet bitiş noktasını etkinleştirin.
* İki Azure Depolama hesabı oluşturun ve yukarıda oluşturulan alt ağdan ağa erişime izin verin.
* Yalnızca depolama hesaplarından birine erişime izin vermek için bir hizmet bitiş noktası ilkesi oluşturun.
* Alt ağa sanal bir makine (VM) dağıtın.
* Alt ağdan izin verilen depolama hesabına erişimi onaylayın.
* Alt ağdan izin verilmeyen depolama hesabına erişimin reddedildiğini onaylayın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Sanal ağ oluşturmadan önce, sanal ağ ve bu makalede oluşturulan diğer tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

[Az ağ vnet oluşturmak](/cli/azure/network/vnet)ile bir alt net ile sanal ağ oluşturun.

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Private \
  --subnet-prefix 10.0.0.0/24
```

## <a name="enable-a-service-endpoint"></a>Hizmet uç noktasını girin 

Bu örnekte, *Microsoft.Storage* için bir hizmet bitiş noktası alt ağ *Özel*için oluşturulur: 

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.0.0/24 \
  --service-endpoints Microsoft.Storage
```

## <a name="restrict-network-access-for-a-subnet"></a>Bir kaynak için ağ erişimini kısıtlama

[az ağ nsg oluşturmak](/cli/azure/network/nsg)ile bir ağ güvenlik grubu oluşturun. Aşağıdaki örnek, *myNsgPrivate*adlı bir ağ güvenlik grubu oluşturur.

```azurecli-interactive
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsgPrivate
```

Ağ güvenlik grubunu [az ağ vnet alt ağı güncelleştirmesi](/cli/azure/network/vnet/subnet)ile *Özel* alt ağla ilişkilendirin. Aşağıdaki örnek, *myNsgPrivate* ağ güvenlik grubunu *Özel* alt ağla ilişkilendirer:

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Private \
  --resource-group myResourceGroup \
  --network-security-group myNsgPrivate
```

az ağ [nsg kuralı oluşturmak](/cli/azure/network/nsg/rule)ile güvenlik kuralları oluşturun. Aşağıdaki kural, Azure Depolama hizmetine atanan genel IP adreslerine giden erişime izin verir: 

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-Storage-All \
  --access Allow \
  --protocol "*" \
  --direction Outbound \
  --priority 100 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Storage" \
  --destination-port-range "*"
```

Her ağ güvenlik grubu birkaç [varsayılan güvenlik kuralı](security-overview.md#default-security-rules)içerir. İzleyen kural, tüm genel IP adreslerine giden erişime izin veren varsayılan güvenlik kuralını geçersiz kılar. Bu `destination-address-prefix "Internet"` seçenek, tüm genel IP adreslerine giden erişimi reddeder. Önceki kural, Azure Depolama'nın genel IP adreslerine erişime izin veren daha yüksek önceliği nedeniyle bu kuralı geçersiz kılar.

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Deny-Internet-All \
  --access Deny \
  --protocol "*" \
  --direction Outbound \
  --priority 110 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Internet" \
  --destination-port-range "*"
```

Aşağıdaki kural, SSH trafiğinin her yerden alt ağa bağlanmasına izin verir. Kural, internetten gelen tüm trafiği engelleyen bir varsayılan güvenlik kuralını geçersiz kılar. SSH'nin alt ağa girmesine izin verilir, böylece bağlantı daha sonraki bir adımda sınanabilir.

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 120 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "VirtualNetwork" \
  --destination-port-range "22"
```

## <a name="restrict-network-access-to-azure-storage-accounts"></a>Azure Depolama hesaplarına ağ erişimini kısıtlama

Bu bölümde, hizmet bitiş noktası üzerinden Sanal ağdaki belirli bir alt ağdaki Azure Depolama hesabının ağ erişimini kısıtlamaya yönelik adımlar listelenir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Az depolama hesabı [oluşturarak](/cli/azure/storage/account)iki Azure depolama hesabı oluşturun.

```azurecli-interactive
storageAcctName1="allowedstorageacc"

az storage account create \
  --name $storageAcctName1 \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2

storageAcctName2="notallowedstorageacc"

az storage account create \
  --name $storageAcctName2 \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2
```

Depolama hesapları oluşturulduktan sonra, depolama hesapları için bağlantı dizesini [az depolama hesabı show-connection-string](/cli/azure/storage/account)ile bir değişkene alın. Bağlantı dizesi, daha sonraki bir adımda dosya paylaşımı oluşturmak için kullanılır.

```azurecli-interactive
saConnectionString1=$(az storage account show-connection-string \
  --name $storageAcctName1 \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)

saConnectionString2=$(az storage account show-connection-string \
  --name $storageAcctName2 \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)
```

<a name="account-key"></a>Daha sonraki bir adımda kullanıldığından, değişkenin içeriğini görüntüleyin ve çıktıda döndürülen **AccountKey** değerini not alın.

```azurecli-interactive
echo $saConnectionString1

echo $saConnectionString2
```

### <a name="create-a-file-share-in-the-storage-account"></a>Depolama hesabında dosya paylaşımı oluşturma

[Az depolama payı oluşturarak](/cli/azure/storage/share)depolama hesabında bir dosya paylaşımı oluşturun. Daha sonraki bir adımda, bu dosya paylaşımı ağ erişimini onaylamak için monte edilir.

```azurecli-interactive
az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString1 > /dev/null

az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString2 > /dev/null
```

### <a name="deny-all-network-access-to-the-storage-account"></a>Depolama hesabına tüm ağ erişimini reddetme

Varsayılan olarak, depolama hesapları herhangi bir ağdaki istemcilerden gelen ağ bağlantılarını kabul eder. Seçili ağlara erişimi sınırlamak için varsayılan eylemi [az depolama hesabı güncelleştirmesiyle](/cli/azure/storage/account) *Reddet* olarak değiştirin. Ağ erişimi reddedildiğinde depolama hesabı hiçbir ağdan erişilebilir olmaz.

```azurecli-interactive
az storage account update \
  --name $storageAcctName1 \
  --resource-group myResourceGroup \
  --default-action Deny

az storage account update \
  --name $storageAcctName2 \
  --resource-group myResourceGroup \
  --default-action Deny
```

### <a name="enable-network-access-from-virtual-network-subnet"></a>Sanal ağ alt ağından ağ erişimini etkinleştirme

Az depolama hesabı ağ kuralı ekle ile *Özel* alt ağdan depolama hesabına ağ [erişimine](/cli/azure/storage/account/network-rule)izin ver.

```azurecli-interactive
az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName1 \
  --vnet-name myVirtualNetwork \
  --subnet Private

az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName2 \
  --vnet-name myVirtualNetwork \
  --subnet Private
```

## <a name="apply-policy-to-allow-access-to-valid-storage-account"></a>Geçerli depolama hesabına erişime izin vermek için ilke uygulayın

Azure Hizmet Bitiş Noktası ilkeleri yalnızca Azure Depolama için kullanılabilir. Bu nedenle, bu örnek kurulum için bu alt ağda *Microsoft.Storage* için Hizmet Bitiş Noktası'nı etkinleştireceğiz.

Hizmet bitiş noktası ilkeleri, hizmet bitiş noktaları üzerinden uygulanır. Bir hizmet bitiş noktası ilkesi oluşturarak başlayacağız. Daha sonra, bu alt ağ için beyaz listeye alınacak Azure Depolama hesapları için bu ilke kapsamındaki ilke tanımlarını oluşturacağız

Hizmet bitiş noktası ilkesi oluşturma

```azurecli-interactive
az network service-endpoint policy create \
  --resource-group myResourceGroup \
  --name mysepolicy \
  --location eastus
```

Kaynak URI'yi bir değişkendeki izin verilen depolama hesabına kaydedin. Aşağıdaki komutu uygulamadan * \<önce, abonelik kimliğinizi>abonelik kimliğinizin* gerçek değeriyle değiştirin.

```azurecli-interactive
$serviceResourceId="/subscriptions/<your-subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/allowedstorageacc"
```

Yukarıdaki Azure Depolama hesabının hizmet bitiş noktası ilkesine izin vermek için bir ilke tanımı & oluşturun

```azurecli-interactive
az network service-endpoint policy-definition create \
  --resource-group myResourceGroup \
  --policy-name mysepolicy \
  --name mypolicydefinition \
  --service "Microsoft.Storage" \
  --service-resources $serviceResourceId
```

Ve önceki adımda oluşturulan hizmet bitiş noktası ilkesini ilişkilendirmek için sanal ağ alt netini güncelleştirin

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --service-endpoints Microsoft.Storage \
  --service-endpoint-policy mysepolicy
```

## <a name="validate-access-restriction-to-azure-storage-accounts"></a>Azure Depolama hesaplarına erişim kısıtlamasını doğrulama

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

Bir depolama hesabına ağ erişimini test etmek için alt ağa bir VM dağıtın.

Az vm oluşturmak ile *Özel* alt net bir [VM oluşturun.](/cli/azure/vm) SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. Oluşturulduktan sonra, döndürülen çıktıdaki **publicIpAddress'e** dikkat edin. Bu adres, daha sonraki bir adımda VM'ye internetten erişmek için kullanılır.

### <a name="confirm-access-to-storage-account"></a>Depolama hesabına erişimi onaylama

*myVmPrivate* VM içine SSH. * \<PublicIpAddress>* *myVmPrivate* VM'nizin genel IP adresiyle değiştirin.

```bash 
ssh <publicIpAddress>
```

Montaj noktası için bir klasör oluşturun:

```bash
sudo mkdir /mnt/MyAzureFileShare1
```

Azure dosya paylaşımını oluşturduğunuz dizine bağla. Aşağıdaki komutu yürütmeden * \<önce, depolama hesabı anahtarı>* **$saConnectionString1'den** *hesap anahtarı* değeriyle değiştirin.

```bash
sudo mount --types cifs //allowedstorageacc.file.core.windows.net/my-file-share /mnt/MyAzureFileShare1 --options vers=3.0,username=allowedstorageacc,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

`user@myVmPrivate:~$` İstemi alırsınız. Azure dosya paylaşımı */mnt/MyAzureFileShare'e*başarıyla monte edilmiştir.

### <a name="confirm-access-is-denied-to-storage-account"></a>Depolama hesabına erişimin reddedildiğini onaylama

Aynı VM *myVmPrivate*itibaren, bir montaj noktası için bir dizin oluşturmak:

```bash
sudo mkdir /mnt/MyAzureFileShare2
```

Azure dosya paylaşımını depolama hesabından *depolama yada* depolama aygıtına monte etmeye çalış. Bu makalede, Ubuntu'nun en son sürümünü dağıtdığınız varsayar. Ubuntu'nun önceki sürümlerini kullanıyorsanız, dosya paylaşımlarını montaj hakkında ek talimatlar için [Linux'ta Mount on'a](../storage/files/storage-how-to-use-files-linux.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bakın. 

Aşağıdaki komutu yürütmeden * \<önce, depolama hesabı-anahtar>* **$saConnectionString2'den** *hesap anahtarı* değeriyle değiştirin.

```bash
sudo mount --types cifs //notallowedstorageacc.file.core.windows.net/my-file-share /mnt/MyAzureFileShare2 --options vers=3.0,username=notallowedstorageacc,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Bu depolama hesabı alt ağa `mount error(13): Permission denied` uyguladığımız hizmet bitiş noktası ilkesinin izin listesinde olmadığından, erişim reddedilir ve bir hata alırsınız. 

SSH oturumundan *myVmPublic* VM'ye çıkın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az grubu silme'yi](/cli/azure) kullanın.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Depolama'ya bir Azure sanal ağ hizmeti bitiş noktası üzerinden bir hizmet bitiş noktası ilkesi uyguladınız. Azure Depolama hesapları ve sanal ağ alt ağından yalnızca belirli depolama hesaplarına sınırlı ağ erişimi (ve dolayısıyla başkalarını reddettiniz). Hizmet bitiş noktası ilkeleri hakkında daha fazla bilgi edinmek için [Hizmet uç noktaları ilkelerine genel bakış'e](virtual-network-service-endpoint-policies-overview.md)bakın.
