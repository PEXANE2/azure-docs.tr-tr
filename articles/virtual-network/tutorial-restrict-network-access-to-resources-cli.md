---
title: PaaS kaynaklarına ağ erişimini kısıtlama - Azure CLI
description: Bu makalede, Azure CLI'yi kullanan sanal ağ hizmeti bitiş noktalarıyla Azure Depolama ve Azure SQL Veritabanı gibi Azure kaynaklarına ağ erişimini nasıl sınırlandırabileceğinizi ve kısıtlamayı öğrenirsiniz.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: f2dcc714bc9052dd51f114e24f0b9bd74b87480c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74186396"
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-cli"></a>Azure CLI'yi kullanarak sanal ağ hizmeti bitiş noktalarıyla PaaS kaynaklarına ağ erişimini kısıtlama

Sanal ağ hizmet uç noktaları bazı Azure hizmet uç noktalarına ağ erişimini bir sanal ağ alt ağı ile sınırlamanıza olanak tanır. Ayrıca, kaynaklara internet erişimini de kaldırabilirsiniz. Hizmet uç noktaları, sanal ağınızdan desteklenen Azure hizmetlerine doğrudan bağlantı sağlar, böylece Azure hizmetlerine erişmek için sanal ağınızın özel adres alanını kullanabilirsiniz. Hizmet uç noktaları aracılığıyla Azure kaynaklarına gönderilen trafik her zaman Microsoft Azure omurga ağı üzerinde kalır. Bu makalede şunları öğreneceksiniz:

* Alt ağ ile sanal ağ oluşturma
* Alt ağ ekleme ve hizmet uç noktasını etkinleştirme
* Azure kaynağı oluşturma ve yalnızca bir alt ağdan ağ erişimine izin verme
* Her alt ağa bir sanal makine (VM) dağıtma
* Bir alt ağdan kaynağa erişimi onaylama
* Bir alt ağdan ve internetten kaynağa erişimin reddedildiğini onaylama

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
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

## <a name="enable-a-service-endpoint"></a>Hizmet uç noktasını girin 

Yalnızca hizmet bitiş noktalarını destekleyen hizmetler için hizmet bitiş noktalarını etkinleştirebilirsiniz. [Az network vnet list endpoint-endpoint-services](/cli/azure/network/vnet)ile Azure'da kullanılabilen hizmet bitiş noktası etkinleştirilmiş hizmetleri görüntüleyin. Aşağıdaki örnek, *eastus* bölgesinde kullanılabilen hizmet sonu etkinleştirilmiş hizmetlerin listesini döndürür. Daha fazla Azure hizmeti hizmet bitiş noktası etkinleştirildikçe, döndürülen hizmetlerin listesi zaman içinde büyür.

```azurecli-interactive
az network vnet list-endpoint-services \
  --location eastus \
  --out table
``` 

[Az ağ vnet subnet oluşturmak](/cli/azure/network/vnet/subnet)ile sanal ağda ek bir alt ağ oluşturun. Bu örnekte, alt ağ için *Microsoft.Storage* için bir hizmet bitiş noktası oluşturulur: 

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24 \
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

## <a name="restrict-network-access-to-a-resource"></a>Bir kaynağa ağ erişimini kısıtlama

Hizmet uç noktaları için etkinleştirilmiş Azure hizmetleri aracılığıyla oluşturulan kaynaklara ağ erişimini kısıtlamak için gereken adımlar, hizmetler arasında farklılık gösterir. Bir hizmete yönelik belirli adımlar için ilgili hizmetin belgelerine bakın. Bu makalenin geri kalanı, örnek olarak bir Azure Depolama hesabının ağ erişimini kısıtlamaya yönelik adımları içerir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Az depolama hesabı [oluşturarak](/cli/azure/storage/account)bir Azure depolama hesabı oluşturun. Yalnızca `<replace-with-your-unique-storage-account-name>` sayılar ve küçük harfler kullanarak, 3-24 karakter arasında, tüm Azure konumlarında benzersiz bir adla değiştirin.

```azurecli-interactive
storageAcctName="<replace-with-your-unique-storage-account-name>"

az storage account create \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2
```

Depolama hesabı oluşturulduktan sonra, depolama hesabı nın bağlantı dizesini [az depolama hesabı show-connection-string'e](/cli/azure/storage/account)sahip bir değişkene alın. Bağlantı dizesi, daha sonraki bir adımda dosya paylaşımı oluşturmak için kullanılır.

```azurecli-interactive
saConnectionString=$(az storage account show-connection-string \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)
```

<a name="account-key"></a>Daha sonraki bir adımda kullanıldığından, değişkenin içeriğini görüntüleyin ve çıktıda döndürülen **AccountKey** değerini not alın.

```azurecli-interactive
echo $saConnectionString
```

### <a name="create-a-file-share-in-the-storage-account"></a>Depolama hesabında dosya paylaşımı oluşturma

[Az depolama payı oluşturarak](/cli/azure/storage/share)depolama hesabında bir dosya paylaşımı oluşturun. Daha sonraki bir adımda, bu dosya paylaşımı ağ erişimini onaylamak için monte edilir.

```azurecli-interactive
az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString > /dev/null
```

### <a name="deny-all-network-access-to-a-storage-account"></a>Depolama hesabına tüm ağ erişimini reddetme

Varsayılan olarak, depolama hesapları herhangi bir ağdaki istemcilerden gelen ağ bağlantılarını kabul eder. Seçili ağlara erişimi sınırlamak için varsayılan eylemi [az depolama hesabı güncelleştirmesiyle](/cli/azure/storage/account) *Reddet* olarak değiştirin. Ağ erişimi reddedildiğinde depolama hesabı hiçbir ağdan erişilebilir olmaz.

```azurecli-interactive
az storage account update \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --default-action Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Bir alt ağdan ağ erişimini etkinleştirme

Az depolama hesabı ağ kuralı ekle ile *Özel* alt ağdan depolama hesabına ağ [erişimine](/cli/azure/storage/account/network-rule)izin ver.

```azurecli-interactive
az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName \
  --vnet-name myVirtualNetwork \
  --subnet Private
```
## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir depolama hesabına ağ erişimini test etmek için her alt ağa bir VM dağıtın.

### <a name="create-the-first-virtual-machine"></a>İlk sanal makineyi oluşturma

Az vm oluşturmak ile *Ortak* alt net bir [VM oluşturun.](/cli/azure/vm) SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra, Azure CLI aşağıdaki örneğe benzer bilgileri gösterir: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmPublic",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Döndürülen çıktıdaki **publicIpAddress'e** dikkat edin. Bu adres, daha sonraki bir adımda VM'ye internetten erişmek için kullanılır.

### <a name="create-the-second-virtual-machine"></a>İkinci sanal makineyi oluşturma

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

## <a name="confirm-access-to-storage-account"></a>Depolama hesabına erişimi onaylama

*myVmPrivate* VM içine SSH. * \<PublicIpAddress>* *myVmPrivate* VM'nizin genel IP adresiyle değiştirin.

```bash 
ssh <publicIpAddress>
```

Montaj noktası için bir klasör oluşturun:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Azure dosya paylaşımını oluşturduğunuz dizine bağla. Aşağıdaki komutu çalıştırmadan `<storage-account-name>` önce, hesap `<storage-account-key>` adı ve bir [depolama hesabı oluştur'da](#create-a-storage-account)aldığınız anahtarla değiştirin.

```bash
sudo mount --types cifs //<storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

`user@myVmPrivate:~$` İstemi alırsınız. Azure dosya paylaşımı */mnt/MyAzureFileShare'e*başarıyla monte edilmiştir.

VM'nin diğer genel IP adreslerine giden bağlantısı olmadığını doğrulayın:

```bash
ping bing.com -c 4
```

*Özel* alt ağ ile ilişkili ağ güvenlik grubu Azure Depolama hizmetine atanan adreslerden başka genel IP adreslerine giden erişime izin vermediği için bir yanıt almazsınız.

SSH oturumundan *myVmPrivate* VM'ye çıkın.

## <a name="confirm-access-is-denied-to-storage-account"></a>Depolama hesabına erişimin reddedildiğini onaylama

*myVmPublic* VM ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. `<publicIpAddress>` *myVmPublic* VM'nizin genel IP adresiyle değiştirin: 

```bash 
ssh <publicIpAddress>
```

Montaj noktası için bir dizin oluşturun:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Azure dosya paylaşımını oluşturduğunuz dizine monte etmeye çalış. Bu makalede, Ubuntu'nun en son sürümünü dağıtdığınız varsayar. Ubuntu'nun önceki sürümlerini kullanıyorsanız, dosya paylaşımlarını montaj hakkında ek talimatlar için [Linux'ta Mount on'a](../storage/files/storage-how-to-use-files-linux.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bakın. Aşağıdaki komutu çalıştırmadan `<storage-account-name>` önce, hesap `<storage-account-key>` adı ve depolama [hesabı oluştur'da](#create-a-storage-account)aldığınız anahtarla değiştirin:

```bash
sudo mount --types cifs //storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

`mount error(13): Permission denied` *MyVmPublic* VM *Ortak* alt ağ içinde dağıtıldı, çünkü erişim reddedildi ve bir hata alırsınız. *Genel* alt ağında Azure Depolama için etkinleştirilmiş bir hizmet uç noktası bulunmaz ve depolama hesabı *Genel* alt ağından değil, yalnızca *Özel* alt ağından ağ erişimine izin verir.

SSH oturumundan *myVmPublic* VM'ye çıkın.

Bilgisayarınızdan, [az depolama paylaşım listesi](/cli/azure/storage/share?view=azure-cli-latest)ile depolama hesabınızdaki hisseleri görüntülemeye çalış. Bir `<account-name>` `<account-key>` [depolama hesabı oluştur'dan](#create-a-storage-account)depolama hesabı adı ve anahtarını değiştirin:

```azurecli-interactive
az storage share list \
  --account-name <account-name> \
  --account-key <account-key>
```

Erişim reddedilir ve bilgisayarınız *MyVirtualNetwork* sanal ağının *Özel* alt ağında olmadığından, bu işlem hatasını *gerçekleştirmek için bu isteği* alma yetkisi yoktur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az grubu silme'yi](/cli/azure) kullanın.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağ alt ağı için bir hizmet bitiş noktası etkinleştirdin. Hizmet uç noktalarının birden fazla Azure hizmeti ile dağıtılmış kaynaklar için etkinleştirilebildiğini öğrendiniz. Bir Azure Depolama hesabı oluşturdunuz ve depolama hesabına ağ erişimini yalnızca bir sanal ağ alt ağındaki kaynaklarla sınırladınız. Hizmet uç noktaları hakkında daha fazla bilgi için bkz. [Hizmet uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md) ve [Alt ağları yönetme](virtual-network-manage-subnet.md).

Hesabınızda birden fazla sanal ağ varsa, her bir sanal ağın içindeki kaynakların birbiriyle iletişim kurabilmesi iki sanal ağı birbirine bağlamak isteyebilirsiniz. Nasıl yapılacağını öğrenmek için [sanal ağları bağla'ya](tutorial-connect-virtual-networks-cli.md)bakın.
