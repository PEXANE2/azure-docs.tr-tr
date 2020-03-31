---
title: Quickstart - Azure CLI'yi kullanarak Azure özel bitiş noktası oluşturma
description: Bu Quickstart'ta Azure özel bitiş noktası hakkında bilgi edinin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: dbcb833e6f8b90cebd3d013e58168558bcd96827
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75459963"
---
# <a name="quickstart-create-a-private-endpoint-using-azure-cli"></a>Hızlı başlatma: Azure CLI'yi kullanarak özel bir bitiş noktası oluşturma
Özel Bitiş Noktası, Azure'daki Özel Bağlantı'nın temel yapı taşıdır. Sanal makineler (VM'ler) gibi Azure kaynaklarının Özel Bağlantı Kaynakları ile özel olarak iletişim kurmasını sağlar. Bu Quickstart'ta, Sanal ağda nasıl VM, Azure CLI kullanarak Özel Bitiş Noktası olan bir SQL Veritabanı Sunucusu'nda nasıl vm oluşturulacağı öğrenilir. Daha sonra, Özel bağlantı kaynağına (bu örnekte özel bir Azure SQL Veritabanı sunucusu) VM'ye erişebilir ve güvenli bir şekilde erişebilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI'yi yerel olarak yüklemeye ve kullanmaya karar verirseniz, bu hızlı başlangıç Azure CLI sürüm 2.0.28 veya sonraki sürümlerini kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için `az --version`çalıştırın. Bilgileri yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli) bakın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Herhangi bir kaynak oluşturabilmek için, Sanal Ağ'ı barındıracak bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Bu örnek, *westcentralus* konumunda *myResourceGroup* adında bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location westcentralus
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

## <a name="create-a-sql-database-server"></a>SQL Veritabanı Sunucusu Oluşturma 
az sql server create komutu ile bir SQL Database Server oluşturun. SQL Server'ınızın adının Azure'da benzersiz olması gerektiğini unutmayın, bu nedenle parantez içinde yer tutucu değerini kendi benzersiz değerinizle değiştirin: 

```azurecli-interactive
# Create a logical server in the resource group 
az sql server create \ 
    --name "myserver"\ 
    --resource-group myResourceGroup \ 
    --location WestUS \ 
    --admin-user "sqladmin" \ 
    --admin-password "CHANGE_PASSWORD_1" 
 
# Create a database in the server with zone redundancy as false 
az sql db create \ 
    --resource-group myResourceGroup  \ 
    --server myserver \ 
    --name mySampleDatabase \ 
    --sample-name AdventureWorksLT \ 
    --edition GeneralPurpose \ 
    --family Gen4 \ 
    --capacity 1 
```

SQL Server ID'nin ```/subscriptions/subscriptionId/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/myserver.``` bir sonraki adımda SQL Server ID'yi kullanacağınız adedine benzediğini unutmayın. 

## <a name="create-the-private-endpoint"></a>Özel Bitiş Noktası'nı oluşturma 
Sanal Ağınızda SQL Veritabanı sunucusu için özel bir bitiş noktası oluşturun: 
```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id "<SQL Server ID>" \  
    --group-ids sqlServer \  
    --connection-name myConnection  
 ```
## <a name="configure-the-private-dns-zone"></a>Özel DNS Bölgesini Yapılandırma 
SQL Database sunucu etki alanı için özel bir DNS Bölgesi oluşturun ve Sanal Ağ ile bir ilişkilendirme bağlantısı oluşturun. 
```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \ 
   --name  "privatelink.database.windows.net" 
az network private-dns link vnet create --resource-group myResourceGroup \ 
   --zone-name  "privatelink.database.windows.net"\ 
   --name MyDNSLink \ 
   --virtual-network myVirtualNetwork \ 
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name myPrivateEndpoint --resource-group myResourceGroup --query 'networkInterfaces[0].id' -o tsv)
 
 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
# Copy the content for privateIPAddress and FQDN matching the SQL server name 
 
 
#Create DNS records 
az network private-dns record-set a create --name myserver --zone-name privatelink.database.windows.net --resource-group myResourceGroup  
az network private-dns record-set a add-record --record-set-name myserver --zone-name privatelink.database.windows.net --resource-group myResourceGroup -a <Private IP Address>
```

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Aşağıdaki gibi internetten VM *myVm* bağlanın:

1. Portalın arama çubuğuna *myVm*girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. **RDP Dosyasını İndir'i**seçin. Azure uzak masaüstü protokolü (*.rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen.rdp* dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adı ve parolayı girin.

        > [!NOTE]
        > VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullanın,** **daha fazla seçenek** > seçmeniz gerekebilir.

1. **Tamam'ı**seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Sertifika uyarısı alırsanız **Evet** veya **Devam et'i**seçin.

1. VM masaüstü göründükten sonra, yerel masaüstünüze geri dönmek için en aza indirin.  

## <a name="access-sql-database-server-privately-from-the-vm"></a>SQL Database Server'a VM'den özel olarak erişin

Bu bölümde, Özel Bitiş Noktası'nı kullanarak VM'den SQL Database Server'a bağlanabilirsiniz.

 1. *myVM*Uzak Masaüstünde, PowerShell'i açın.
 2. nslookup girin  myserver.database.windows.net Buna benzer bir ileti alırsınız: 

```
      Server:  UnKnown 
      Address:  168.63.129.16 
      Non-authoritative answer: 
      Name:    myserver.privatelink.database.windows.net 
      Address:  10.0.0.5 
      Aliases:  myserver.database.windows.net 
```
 3. SQL Server Management Studio'yı yükleyin 
 4. Sunucuya Bağlan'da şu bilgileri girin veya seçin: Sunucu türü: Veritabanı Altyapısı'nı seçin.
 Sunucu adı: myserver.database.windows.net Kullanıcı Adını seçin: Oluşturma sırasında sağlanan bir kullanıcı adı girin.
 Parola: Oluşturma sırasında sağlanan bir parola girin.
 Parolayı hatırla: Evet'i seçin.
 
 5. **Bağlan**’ı seçin.
 6. Sol menüden **Veritabanlarına** göz atın.
 7. (İsteğe bağlı olarak) *Veritabanımdan* bilgi oluşturma veya sorgulama
 8. Uzak masaüstü bağlantısını *myVm'e*kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Artık gerekmediğinde, kaynak grubunu ve sahip olduğu tüm kaynakları kaldırmak için az group delete'i kullanabilirsiniz: 

```azurecli-interactive
az group delete --name myResourceGroup --yes 
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Özel Bağlantı](private-link-overview.md) hakkında daha fazla bilgi edinin
