---
title: Azure Cosmos hesabı için sanal ağ tabanlı erişimi yapılandırma
description: Bu belge, Azure Cosmos DB için bir sanal ağ hizmeti bitiş noktası ayarlamak için gereken adımları açıklar.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mjbrown
ms.openlocfilehash: 442623880c1b95f3d7e038ae44832b74853d2c4a
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366233"
---
# <a name="configure-access-from-virtual-networks-vnet"></a>Sanal ağlardan erişimi yapılandırma (VNet)

Azure Cosmos DB hesaplarını, yalnızca Azure sanal ağının belirli bir alt ağından erişime izin verecek şekilde yapılandırabilirsiniz. Sanal ağdaki bir alt ağdan gelen bağlantılarla azure cosmos DB hesabına erişimi sınırlamak için:

1. Alt ağ ve sanal ağ kimliğini Azure Cosmos DB'ye göndermek için alt ağı etkinleştirin. Azure Cosmos DB için belirli bir alt ağda bir hizmet bitiş noktası sağlayarak bunu başarabilirsiniz.

1. Alt ağı hesaba erişilebilen bir kaynak olarak belirtmek için Azure Cosmos DB hesabına bir kural ekleyin.

> [!NOTE]
> Azure Cosmos DB hesabınız için bir hizmet bitiş noktası bir alt ağda etkinleştirildiğinde, Azure Cosmos DB'ye ulaşan trafiğin kaynağı ortak bir IP'den sanal ağa ve alt ağa geçer. Trafik geçişi, bu alt ağdan erişilen tüm Azure Cosmos DB hesapları için geçerlidir. Azure Cosmos DB hesaplarınızın bu alt ağa izin vermek için IP tabanlı bir güvenlik duvarı varsa, hizmet etkin leştirilmiş alt ağdan gelen istekler artık IP güvenlik duvarı kurallarıyla eşleşmez ve reddedilir.
>
> Daha fazla bilgi edinmek için, bir IP güvenlik duvarı kuralından bu makalenin [sanal ağ erişim denetim listesi bölümüne Geçiş'te](#migrate-from-firewall-to-vnet) özetlenen adımlara bakın.

Aşağıdaki bölümlerde, bir Azure Cosmos DB hesabı için sanal ağ hizmeti bitiş noktasının nasıl yapılandırılabildiğini açıklanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configure-a-service-endpoint-by-using-the-azure-portal"></a><a id="configure-using-portal"></a>Azure portalını kullanarak bir hizmet bitiş noktasını yapılandırma

### <a name="configure-a-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Varolan bir Azure sanal ağı ve alt ağı için bir hizmet bitiş noktası yapılandırma

1. Tüm **kaynaklar** bıçağından, güvenliğini sağlamak istediğiniz Azure Cosmos DB hesabını bulun.

1. Ayarlar menüsünden **Güvenlik Duvarları ve sanal ağları** seçin ve **Seçili ağlardan**erişime izin vermeyi seçin.

1. **Sanal ağlar**altında varolan bir sanal ağın alt ağına erişim sağlamak için **varolan Azure sanal ağ ekle'yi**seçin.

1. Azure sanal ağı eklemek istediğiniz **Abonelik'i** seçin. Azure Cosmos DB hesabınıza erişim sağlamak istediğiniz Azure **Sanal ağları** ve **Alt Ağları'nı** seçin. Ardından, "Microsoft.AzureCoscosCosDB" için hizmet bitiş noktalarına sahip seçili ağları etkinleştirme yi **seçin.** Tamamlandığında **Ekle'yi**seçin.

   ![Sanal ağ ve alt ağı seçin](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png)

1. Azure Cosmos DB hesabı sanal ağdan erişim için etkinleştirildikten sonra, yalnızca seçilen bu alt ağdan gelen trafiğe izin verir. Eklediğiniz sanal ağ ve alt ağ aşağıdaki ekran görüntüsünde gösterildiği gibi görünmelidir:

   ![Sanal ağ ve alt ağ başarıyla yapılandırıldı](./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

> [!NOTE]
> Sanal ağ hizmeti uç noktalarını etkinleştirmek için aşağıdaki abonelik izinlerine ihtiyacınız vardır:
>   * Sanal ağ ile abonelik: Ağ katkıda bulunan
>   * Azure Cosmos DB hesabı yla abonelik: DocumentDB hesap katılımcısı
>   * Sanal ağınız ve Azure Cosmos DB hesabınız farklı aboneliklerdeyse, sanal ağa `Microsoft.DocumentDB` sahip aboneliğin de kayıtlı kaynak sağlayıcısı olduğundan emin olun. Bir kaynak sağlayıcısı kaydetmek için [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md) makalesine bakın.

Aboneliği kaynak sağlayıcısına kaydetme yönergeleri aşağıda veda edecektir.

### <a name="configure-a-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Yeni bir Azure sanal ağı ve alt ağı için bir hizmet bitiş noktası yapılandırma

1. Tüm **kaynaklar** bıçağından, güvenliğini sağlamak istediğiniz Azure Cosmos DB hesabını bulun.  

1. Ayarlar menüsünden **Güvenlik Duvarları ve Azure sanal ağlarını** seçin ve **Seçili ağlardan**erişime izin vermeyi seçin.  

1. **Sanal ağlar**altında yeni bir Azure sanal ağına erişim sağlamak için yeni sanal **ağ ekle'yi**seçin.  

1. Yeni bir sanal ağ oluşturmak için gereken ayrıntıları sağlayın ve ardından **Oluştur'u**seçin. Alt ağ, "Microsoft.AzureCoscosCosDB" etkin bir hizmet bitiş noktası ile oluşturulur.

   ![Yeni bir sanal ağ için sanal ağ ve alt ağ seçin](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

Azure Cosmos DB hesabınız Azure Bilişsel Arama gibi diğer Azure hizmetleri tarafından kullanılıyorsa veya Akış analitiği veya Power BI'den erişilirse, **küresel Azure veri merkezlerinden bağlantıları kabul**et seçeneğini seçerek erişime izin verirsiniz.

Portaldan Azure Cosmos DB ölçümlerine erişebildiğinizden emin olmak için **Azure portalı seçeneklerinden erişime izin verme'yi** etkinleştirmeniz gerekir. Bu seçenekler hakkında daha fazla bilgi edinmek için [bir IP güvenlik duvarı makalesini Yapılandır'a](how-to-configure-firewall.md) bakın. Erişimi etkinleştirdikten sonra, ayarları kaydetmek için **Kaydet'i** seçin.

## <a name="remove-a-virtual-network-or-subnet"></a><a id="remove-vnet-or-subnet"></a>Sanal ağ veya alt ağı kaldırma

1. Tüm **kaynaklar** bıçağından, hizmet bitiş noktaları atadığınız Azure Cosmos DB hesabını bulun.  

1. Ayarlar menüsünden **Güvenlik Duvarları ve sanal ağları** seçin.  

1. Sanal ağ veya alt ağ kuralını kaldırmak için sanal ağ veya alt ağın yanındaki **...** 'u seçin ve **Kaldır'ı**seçin.

   ![Sanal ağı kaldırma](./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png)

1. Değişikliklerinizi uygulamak için **Kaydet**’i seçin.

## <a name="configure-a-service-endpoint-by-using-azure-powershell"></a><a id="configure-using-powershell"></a>Azure PowerShell'i kullanarak bir hizmet bitiş noktasını yapılandırma

> [!NOTE]
> PowerShell veya Azure CLI'yi kullanırken, sadece eklenmesi gerekenler ekiyle değil, parametrelerdeki IP filtrelerinin ve sanal ağ ALA'larının tam listesini belirttiğinden emin olun.

Azure PowerShell'i kullanarak bir hizmet bitiş noktasını Azure Cosmos DB hesabına yapılandırmak için aşağıdaki adımları kullanın:  

1. [Azure PowerShell'i](https://docs.microsoft.com/powershell/azure/install-Az-ps) yükleyin ve [oturum açın.](https://docs.microsoft.com/powershell/azure/authenticate-azureps)  

1. Sanal bir ağın varolan bir alt ağı için hizmet bitiş noktasını etkinleştirin.  

   ```powershell
   $resourceGroupName = "<Resource group name>"
   $vnetName = "<Virtual network name>"
   $subnetName = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"
   $serviceEndpoint = "Microsoft.AzureCosmosDB"

   Get-AzVirtualNetwork `
      -ResourceGroupName $resourceGroupName `
      -Name $vnetName | Set-AzVirtualNetworkSubnetConfig `
      -Name $subnetName `
      -AddressPrefix $subnetPrefix `
      -ServiceEndpoint $serviceEndpoint | Set-AzVirtualNetwork
   ```

1. Sanal ağ bilgilerini alın.

   ```powershell
   $vnet = Get-AzVirtualNetwork `
      -ResourceGroupName $resourceGroupName `
      -Name $vnetName

   $subnetId = $vnet.Id + "/subnets/" + $subnetName
   ```

1. Cosmos DB Sanal Ağ Kuralı Hazırlama

   ```powershell
   $vnetRule = New-AzCosmosDBVirtualNetworkRule `
      -Id $subnetId
   ```

1. Azure Cosmos DB hesap özelliklerini yeni Virtual Network bitiş noktası yapılandırmasıyla güncelleştirin: 

   ```powershell
   $accountName = "<Cosmos DB account name>"

   Update-AzCosmosDBAccount `
      -ResourceGroupName $resourceGroupName `
      -Name $accountName `
      -EnableVirtualNetwork $true `
      -VirtualNetworkRuleObject @($vnetRule)
   ```

1. Azure Cosmos DB hesabınızın önceki adımda yapılandırdığınız sanal ağ hizmeti bitiş noktasıyla güncelleştirilip güncelleştirileni doğrulamak için aşağıdaki komutu çalıştırın:

   ```powershell
   $account = Get-AzCosmosDBAccount `
      -ResourceGroupName $resourceGroupName `
      -Name $accountName

   $account.IsVirtualNetworkFilterEnabled
   $account.VirtualNetworkRules
   ```

## <a name="configure-a-service-endpoint-by-using-the-azure-cli"></a><a id="configure-using-cli"></a>Azure CLI'yi kullanarak bir hizmet bitiş noktasını yapılandırma

Azure Cosmos hesapları, alt ağ zaten onlar için yapılandırılmışsa, daha sonra oluşturulduklarında veya güncelleştirildiklerinde hizmet bitiş noktaları için yapılandırılabilir. Hizmet bitiş noktaları, alt ağ henüz onlar için yapılandırılmamış olan Cosmos hesabında da etkinleştirilebilir ve alt ağ daha sonra yapılandırıldığında çalışmaya başlar. Bu esneklik, hem Cosmos hesabına hem de sanal ağ kaynaklarına erişimi olmayan yöneticilerin yapılandırmalarını birbirinden bağımsız hale getirmelerine olanak tanır.

### <a name="create-a-new-cosmos-account-and-connect-it-to-a-back-end-subnet-for-a-new-virtual-network"></a>Yeni bir Cosmos hesabı oluşturun ve yeni bir sanal ağ için arka uç alt ağına bağlayın

Bu örnekte sanal ağ ve alt ağ, oluşturulduklarında her ikisi için de etkinleştirilen hizmet uç noktalarıyla oluşturulur.

```azurecli-interactive
# Create an Azure Cosmos Account with a service endpoint connected to a backend subnet

# Resource group and Cosmos account variables
resourceGroupName='MyResourceGroup'
location='West US 2'
accountName='mycosmosaccount'

# Variables for a new Virtual Network with two subnets
vnetName='myVnet'
frontEnd='FrontEnd'
backEnd='BackEnd'

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a virtual network with a front-end subnet
az network vnet create \
   -n $vnetName \
   -g $resourceGroupName \
   --address-prefix 10.0.0.0/16 \
   --subnet-name $frontEnd \
   --subnet-prefix 10.0.1.0/24

# Create a back-end subnet with service endpoints enabled for Cosmos DB
az network vnet subnet create \
   -n $backEnd \
   -g $resourceGroupName \
   --address-prefix 10.0.2.0/24 \
   --vnet-name $vnetName \
   --service-endpoints Microsoft.AzureCosmosDB

svcEndpoint=$(az network vnet subnet show -g $resourceGroupName -n $backEnd --vnet-name $vnetName --query 'id' -o tsv)

# Create a Cosmos DB account with default values and service endpoints
az cosmosdb create \
   -n $accountName \
   -g $resourceGroupName \
   --enable-virtual-network true \
   --virtual-network-rules $svcEndpoint
```

### <a name="connect-and-configure-a-cosmos-account-to-a-back-end-subnet-independently"></a>Bir Cosmos hesabını arka uç alt ağına bağımsız olarak bağlayın ve yapılandırır

Bu örnek, bir Azure Cosmos hesabının alt ağın hizmet bitiş noktaları için henüz yapılandırılmadığını varolan yeni bir sanal ağa nasıl bağlandığını göstermek için tasarlanmıştır. Bu `--ignore-missing-vnet-service-endpoint` parametre kullanılarak yapılır. Bu, sanal ağın alt ağına yapılandırma tamamlanmadan önce Cosmos hesabının yapılandırmasının hatasız olarak tamamlanmasını sağlar. Alt ağ yapılandırması tamamlandıktan sonra, Cosmos hesabına yapılandırılan alt ağ üzerinden erişilebilir.

```azurecli-interactive
# Create an Azure Cosmos Account with a service endpoint connected to a backend subnet
# that is not yet enabled for service endpoints.

# Resource group and Cosmos account variables
resourceGroupName='MyResourceGroup'
location='West US 2'
accountName='mycosmosaccount'

# Variables for a new Virtual Network with two subnets
vnetName='myVnet'
frontEnd='FrontEnd'
backEnd='BackEnd'

# Create a resource group
az group create -n $resourceGroupName -l $location

# Create a virtual network with a front-end subnet
az network vnet create \
   -n $vnetName \
   -g $resourceGroupName \
   --address-prefix 10.0.0.0/16 \
   --subnet-name $frontEnd \
   --subnet-prefix 10.0.1.0/24

# Create a back-end subnet but without configuring service endpoints (--service-endpoints Microsoft.AzureCosmosDB)
az network vnet subnet create \
   -n $backEnd \
   -g $resourceGroupName \
   --address-prefix 10.0.2.0/24 \
   --vnet-name $vnetName

svcEndpoint=$(az network vnet subnet show -g $resourceGroupName -n $backEnd --vnet-name $vnetName --query 'id' -o tsv)

# Create a Cosmos DB account with default values
az cosmosdb create -n $accountName -g $resourceGroupName

# Add the virtual network rule but ignore the missing service endpoint on the subnet
az cosmosdb network-rule add \
   -n $accountName \
   -g $resourceGroupName \
   --virtual-network $vnetName \
   --subnet svcEndpoint \
   --ignore-missing-vnet-service-endpoint true

read -p'Press any key to now configure the subnet for service endpoints'

az network vnet subnet update \
   -n $backEnd \
   -g $resourceGroupName \
   --vnet-name $vnetName \
   --service-endpoints Microsoft.AzureCosmosDB
```

## <a name="migrating-from-an-ip-firewall-rule-to-a-virtual-network-acl"></a><a id="migrate-from-firewall-to-vnet"></a>IP güvenlik duvarı kuralından sanal ağ ACL'ye geçiş

Bir Azure Cosmos DB hesabını IP güvenlik duvarı kurallarını kullanmaktan sanal ağ hizmeti bitiş noktalarını kullanmaya geçirmek için aşağıdaki adımları kullanın.

Bir Azure Cosmos DB hesabı bir alt ağ için hizmet bitiş noktası için yapılandırıldıktan sonra, bu alt ağdan gelen istekler kaynak genel IP adresi yerine sanal ağ ve alt net kaynak bilgileriyle Azure Cosmos DB'ye gönderilir. Bu istekler artık Azure Cosmos DB hesabında yapılandırılan bir IP filtresiyle eşleşmez, bu nedenle kapalı kalma süresini önlemek için aşağıdaki adımların gerekli olması gerekir.

İşleme geçmeden önce, "Sanal ağın varolan bir alt ağı için hizmet bitiş noktasını etkinleştirin" adlı üst düzey ekibe göre sanal ağ ve alt ağdaki Azure Cosmos DB hizmet bitiş noktasını etkinleştirin.

1. Sanal ağ ve alt ağ bilgilerini alın:

   ```powershell
   $resourceGroupName = "myResourceGroup"
   $accountName = "mycosmosaccount"
   $vnetName = "myVnet"
   $subnetName = "mySubnet"

   $vnet = Get-AzVirtualNetwork `
      -ResourceGroupName $resourceGroupName `
      -Name $vnetName

   $subnetId = $vnet.Id + "/subnets/" + $subnetName
   ```

1. Azure Cosmos DB hesabı için yeni bir Sanal Ağ kuralı nesnesi hazırlayın:

   ```powershell
   $vnetRule = New-AzCosmosDBVirtualNetworkRule `
      -Id $subnetId
   ```

1. Alt ağdan hizmet bitiş noktası erişimini etkinleştirmek için Azure Cosmos DB hesabını güncelleştirin:

   ```powershell
   Update-AzCosmosDBAccount `
      -ResourceGroupName $resourceGroupName `
      -Name $accountName `
      -EnableVirtualNetwork $true `
      -VirtualNetworkRuleObject @($vnetRule)
   ```

1. Alt ağdan erişilen tüm Azure Cosmos DB hesapları için önceki adımları yineleyin.

1. Azure Cosmos DB hesabının Güvenlik Duvarı kurallarından alt ağ için IP güvenlik duvarı kuralını kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB için bir güvenlik duvarı yapılandırmak için [Güvenlik Duvarı destek](firewall-support.md) makalesine bakın.
