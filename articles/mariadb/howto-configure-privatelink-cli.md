---
title: MariaDB için Azure veritabanı (Önizleme) CLı kurulum yöntemi için özel bağlantı
description: Azure CLı 'dan MariaDB için Azure veritabanı için özel bağlantıyı yapılandırma hakkında bilgi edinin
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 5891662a2176fe110c4fec844d2335a8f1c9d500
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75974198"
---
# <a name="create-and-manage-private-link-for-azure-database-for-mariadb-preview-using-cli"></a>CLı kullanarak MariaDB için Azure veritabanı (Önizleme) için özel bağlantı oluşturma ve yönetme

Özel uç nokta, Azure 'da özel bağlantı için temel yapı taşdır. Sanal makineler (VM) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasına olanak sağlar. Bu makalede, Azure CLı 'yı kullanarak Azure sanal ağında bir VM oluşturma ve Azure özel uç noktası ile MariaDB sunucusu için Azure veritabanı oluşturma hakkında bilgi edineceksiniz.

> [!NOTE]
> Bu özellik, MariaDB için Azure veritabanı 'nın Genel Amaçlı ve bellek için Iyileştirilmiş fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:

- [MariaDB sunucusu Için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLı 'yı yüklemek ve kullanmak isterseniz, bu hızlı başlangıç, Azure CLı sürüm 2.0.28 veya sonraki bir sürümünü kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için `az --version`çalıştırın. Bkz. Install veya Upgrade Info for [Azure CLI](/cli/azure/install-azure-cli) .

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Herhangi bir kaynak oluşturabilmeniz için önce sanal ağı barındırmak üzere bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Bu örnek *westeurope* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-virtual-network"></a>Sanal Ağ Oluştur
[Az Network VNET Create](/cli/azure/network/vnet)komutuyla bir sanal ağ oluşturun. Bu örnek, *Mysubnet*adlı bir alt ağ ile *myVirtualNetwork* adlı varsayılan bir sanal ağ oluşturur:

```azurecli-interactive
az network vnet create \
 --name myVirtualNetwork \
 --resource-group myResourceGroup \
 --subnet-name mySubnet
```

## <a name="disable-subnet-private-endpoint-policies"></a>Alt ağ özel uç nokta ilkelerini devre dışı bırak 
Azure, bir sanal ağ içindeki bir alt ağa kaynak dağıtır, bu nedenle özel uç nokta ağ ilkelerini devre dışı bırakmak için alt ağ oluşturmanız veya güncelleştirmeniz gerekir. [Az Network VNET subnet Update](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update)Ile *mysubnet* adlı bir alt ağ yapılandırmasını güncelleştirin:

```azurecli-interactive
az network vnet subnet update \
 --name mySubnet \
 --resource-group myResourceGroup \
 --vnet-name myVirtualNetwork \
 --disable-private-endpoint-network-policies true
```
## <a name="create-the-vm"></a>Sanal makine oluşturma 
Az VM Create ile bir VM oluşturun. İstendiğinde, sanal makine için oturum açma kimlik bilgileri olarak kullanılacak bir parola girin. Bu örnek, *myvm*ADLı bir VM oluşturur: 
```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image Win2019Datacenter
```
 VM 'nin genel IP adresini aklınızda edin. Sonraki adımda İnternet 'ten VM 'ye bağlanmak için bu adresi kullanacaksınız.

## <a name="create-an-azure-database-for-mariadb-server"></a>MariaDB için Azure Veritabanı sunucusu oluşturma 
Az MariaDB Server Create komutuyla bir MariaDB için Azure veritabanı oluşturun. MariaDB sunucunuzun adının Azure genelinde benzersiz olması gerektiğini unutmayın. bu nedenle, yer tutucu değerini köşeli ayraç içinde kendi benzersiz bir değer ile değiştirin: 

```azurecli-interactive
# Create a logical server in the resource group 
az mariadb server create \
--name mydemoserver \
--resource-group myResourcegroup \
--location westeurope \
--admin-user mylogin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2
```

MariaDB sunucu KIMLIĞI, bir sonraki adımda MariaDB sunucu KIMLIĞINI kullanacaksınız ```/subscriptions/subscriptionId/resourceGroups/myResourceGroup/providers/Microsoft.DBforMariaDB/servers/servername.``` benzerdir. 

## <a name="create-the-private-endpoint"></a>Özel uç nokta oluşturma 
Sanal ağınızdaki MariaDB sunucusu için özel bir uç nokta oluşturun: 
```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id "<MariaDB Server ID>" \  
    --group-ids mariadbServer \  
    --connection-name myConnection  
 ```

## <a name="configure-the-private-dns-zone"></a>Özel DNS bölgesini yapılandırma 
Marıdb sunucu etki alanı için bir Özel DNS bölgesi oluşturun ve sanal ağla bir ilişki bağlantısı oluşturun. 
```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \ 
   --name  "privatelink.database.azure.com" 
az network private-dns link vnet create --resource-group myResourceGroup \ 
   --zone-name  "privatelink.database.azure.com"\ 
   --name MyDNSLink \ 
   --virtual-network myVirtualNetwork \ 
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name myPrivateEndpoint --resource-group myResourceGroup --query 'networkInterfaces[0].id' -o tsv)
 
 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
# Copy the content for privateIPAddress and FQDN matching the Azure database for MariaDB name 
 
 
#Create DNS records 
az network private-dns record-set a create --name mydemoserver --zone-name privatelink.database.azure.com --resource-group myResourceGroup  
az network private-dns record-set a add-record --record-set-name mydemoserver --zone-name privatelink.database.windows.net --resource-group myResourceGroup -a <Private IP Address>
```

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Aşağıdaki gibi, internet *'ten gelen VM VM* 'sine bağlanın:

1. Portalın arama çubuğunda *Myvm*' i girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. Seçin **RDP dosyasını indir**. Azure bir Uzak Masaüstü Protokolü ( *. rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen. rdp * dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

        > [!NOTE]
        > VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için **farklı bir hesap kullanmak** > **daha fazla seçenek** belirlemeniz gerekebilir.

1. **Tamam**’ı seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

1. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.  

## <a name="access-the-mariadb-server-privately-from-the-vm"></a>MariaDB sunucusuna VM 'den özel olarak erişin

1.  *Myvm*uzak masaüstünde PowerShell ' i açın.

2.  `nslookup mydemoserver.mariadb.privatelink.database.azure.com` yazın. 

    Şuna benzer bir ileti alacaksınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemoserver.mariadb.privatelink.database.azure.com
    Address:  10.1.3.4

3. Test the private link connection for the MariaDB server using any available client. In the example below I have used [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-installing-windows.html) to do the operation.

4. In **New connection**, enter or select this information:

    | Setting | Value |
    | ------- | ----- |
    | Connection Name| Select the connection name of your choice.|
    | Hostname | Select *mydemoserver.mariadb.privatelink.database.azure.com* |
    | Username | Enter username as *username@servername* which is provided during the MariaDB server creation. |
    | Password | Enter a password provided during the MariaDB server creation. |
    ||

5. Select **Test Connection** or **OK**.

6. (Optionally) Browse databases from left menu and Create or query information from the MariaDB database

8. Close the remote desktop connection to myVm.

## Clean up resources 
When no longer needed, you can use az group delete to remove the resource group and all the resources it has: 

```azurecli-interactive
az group delete --name myResourceGroup --yes 
```

## <a name="next-steps"></a>Sonraki adımlar
[Azure özel uç noktası nedir?](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) hakkında daha fazla bilgi edinin