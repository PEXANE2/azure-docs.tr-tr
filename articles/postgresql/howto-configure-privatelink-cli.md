---
title: Özel Bağlantı - Azure CLI - PostgreSQL için Azure Veritabanı - Tek sunucu
description: Azure CLI'den PostgreSQL- Tek sunucu için Azure Veritabanı için özel bağlantıyı nasıl yapılandırılayarılamayı öğrenin
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: a6baf8b4609382be4a5a31d12cac581da2c17de6
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011676"
---
# <a name="create-and-manage-private-link-for-azure-database-for-postgresql---single-server-using-cli"></a>PostgreSQL için Azure Veritabanı için Özel Bağlantı Oluşturma ve Yönetme - CLI kullanarak tek sunucu

Özel Bitiş Noktası, Azure'daki özel bağlantının temel yapı taşıdır. Sanal Makineler (VM' ler) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasını sağlar. Bu makalede, Azure Sanal Ağ'da VM ve Azure özel bitiş noktasına sahip PostgreSQL Tek sunucu için bir Azure Veritabanı oluşturmak için Azure CLI'yi nasıl kullanacağınızı öğreneceksiniz.

> [!NOTE]
> Bu özellik, PostgreSQL için Azure Veritabanı - Tek sunucunun Genel Amaç ve Bellek Optimize edilmiş fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:

- [PostgreSQL sunucusu ve veritabanı için](quickstart-create-server-database-azure-cli.md)bir Azure Veritabanı.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI'yi yerel olarak yüklemeye ve kullanmaya karar verirseniz, bu hızlı başlangıç Azure CLI sürüm 2.0.28 veya sonraki sürümlerini kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için `az --version`çalıştırın. Bilgileri yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli) bakın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Herhangi bir kaynak oluşturabilmek için, Sanal Ağ'ı barındıracak bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Bu örnek, *westeurope* konumunda *myResourceGroup* adında bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-virtual-network"></a>Sanal Ağ Oluşturma
[az ağ vnet oluşturmak](/cli/azure/network/vnet)ile sanal ağ oluşturun. Bu örnek, *mySubnet*adlı bir alt ağ ile *myVirtualNetwork* adlı varsayılan bir Sanal Ağ oluşturur:

```azurecli-interactive
az network vnet create \
 --name myVirtualNetwork \
 --resource-group myResourceGroup \
 --subnet-name mySubnet
```

## <a name="disable-subnet-private-endpoint-policies"></a>Subnet özel bitiş noktası ilkelerini devre dışı 
Azure kaynakları sanal ağdaki bir alt ağa dağıtır, bu nedenle özel uç nokta ağ ilkelerini devre dışı kılabilir şekilde alt ağı oluşturmanız veya güncelleştirmeniz gerekir. [Az ağ vnet subnet güncelleştirmesi](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update)ile *mySubnet* adlı bir alt net yapılandırması güncelleyin:

```azurecli-interactive
az network vnet subnet update \
 --name mySubnet \
 --resource-group myResourceGroup \
 --vnet-name myVirtualNetwork \
 --disable-private-endpoint-network-policies true
```
## <a name="create-the-vm"></a>Sanal makine oluşturma 
Az vm oluşturmak ile bir VM oluşturun. İstendiğinde, VM için oturum açma kimlik bilgileri olarak kullanılacak bir parola sağlayın. Bu örnek *myVm*adlı bir VM oluşturur: 
```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image Win2019Datacenter
```
 VM'nin genel IP adresine dikkat edin. Bir sonraki adımda internetten VM'ye bağlanmak için bu adresi kullanacaksınız.

## <a name="create-an-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı Oluşturma - Tek sunucu 
Az postgres sunucusu oluşturma komutu ile PostgreSQL için bir Azure Veritabanı oluşturun. PostgreSQL Sunucunuzun adının Azure'da benzersiz olması gerektiğini unutmayın, bu nedenle parantez içinde yer tutucu değerini kendi benzersiz değerinizle değiştirin: 

```azurecli-interactive
# Create a logical server in the resource group 
az postgres server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location westeurope \
--admin-user mylogin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2
```

PostgreSQL Server ID'nin ```/subscriptions/subscriptionId/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/servername.``` bir sonraki adımda PostgreSQL Server ID'yi kullanacağınız gibi olduğunu unutmayın. 

## <a name="create-the-private-endpoint"></a>Özel Bitiş Noktası'nı oluşturma 
Sanal Ağınızda PostgreSQL sunucusu için özel bir bitiş noktası oluşturun: 
```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id "<PostgreSQL Server ID>" \  
    --group-ids postgresqlServer \  
    --connection-name myConnection  
 ```

## <a name="configure-the-private-dns-zone"></a>Özel DNS Bölgesini Yapılandırma 
PostgreSQL sunucu etki alanı için özel bir DNS Bölgesi oluşturun ve Sanal Ağ ile bir ilişkilendirme bağlantısı oluşturun. 
```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \ 
   --name  "privatelink.postgres.database.azure.com" 
az network private-dns link vnet create --resource-group myResourceGroup \ 
   --zone-name  "privatelink.postgres.database.azure.com"\ 
   --name MyDNSLink \ 
   --virtual-network myVirtualNetwork \ 
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name myPrivateEndpoint --resource-group myResourceGroup --query 'networkInterfaces[0].id' -o tsv)
 
 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
# Copy the content for privateIPAddress and FQDN matching the Azure database for PostgreSQL name 
 
 
#Create DNS records 
az network private-dns record-set a create --name myserver --zone-name privatelink.postgres.database.azure.com --resource-group myResourceGroup  
az network private-dns record-set a add-record --record-set-name myserver --zone-name privatelink.postgres.database.azure.com --resource-group myResourceGroup -a <Private IP Address>
```

> [!NOTE] 
> Müşteri DNS ayarındaki FQDN, yapılandırılan özel IP'ye çözüm vermez. [Burada](../dns/dns-operations-recordsets-portal.md)gösterildiği gibi yapılandırılmış FQDN için bir DNS bölgesi kurmanız gerekecektir.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Aşağıdaki gibi internetten VM *myVm* bağlanın:

1. Portalın arama çubuğuna *myVm*girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. **RDP Dosyasını İndir'i**seçin. Azure uzak masaüstü protokolü (*.rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. *İndirilen.rdp* dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adı ve parolayı girin.

        > [!NOTE]
        > VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullanın,** **daha fazla seçenek** > seçmeniz gerekebilir.

1. **Tamam'ı**seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Sertifika uyarısı alırsanız **Evet** veya **Devam et'i**seçin.

1. VM masaüstü göründükten sonra, yerel masaüstünüze geri dönmek için en aza indirin.  

## <a name="access-the-postgresql-server-privately-from-the-vm"></a>PostgreSQL sunucusuna VM'den özel olarak erişin

1.  *myVM*Uzak Masaüstünde, PowerShell'i açın.

2.  `nslookup mydemopostgresserver.privatelink.postgres.database.azure.com` yazın. 

    Buna benzer bir ileti alırsınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemopostgresserver.privatelink.postgres.database.azure.com
    Address:  10.1.3.4
    ```

3. Mevcut istemcileri kullanarak PostgreSQL sunucusunun özel bağlantı bağlantısını test edin. Aşağıdaki örnekte işlemi yapmak için [Azure Data stüdyosunu](https://docs.microsoft.com/sql/azure-data-studio/download?view=sql-server-ver15) kullandım.

4. **Yeni bağlantıda,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sunucu türü| **PostgreSQL'i**seçin.|
    | Sunucu adı| *mydemopostgresserver.privatelink.postgres.database.azure.com'yi* seçin |
    | Kullanıcı adı | PostgreSQL username@servername sunucu oluşturma sırasında sağlanan kullanıcı adını girin. |
    |Parola |PostgreSQL sunucu oluşturma sırasında sağlanan bir parola girin. |
    |SSL|**Gerekli'yi**seçin.|
    ||

5. Bağlan’ı seçin.

6. Sol menüden veritabanlarına göz atın.

7. (İsteğe bağlı olarak) PostgreSQL sunucusundan bilgi oluşturun veya sorgula.

8. Uzak masaüstü bağlantısını myVm'e kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Artık gerekmediğinde, kaynak grubunu ve sahip olduğu tüm kaynakları kaldırmak için az group delete'i kullanabilirsiniz: 

```azurecli-interactive
az group delete --name myResourceGroup --yes 
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure özel bitiş noktası nedir](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) hakkında daha fazla bilgi edinin
