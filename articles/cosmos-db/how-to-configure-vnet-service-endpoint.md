---
title: Azure Cosmos hesabı için sanal ağ tabanlı erişimi yapılandırma
description: Bu belgede Azure Cosmos DB için bir sanal ağ hizmeti uç noktası ayarlamak için gereken adımlar açıklanmaktadır.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mjbrown
ms.openlocfilehash: 442623880c1b95f3d7e038ae44832b74853d2c4a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80366233"
---
# <a name="configure-access-from-virtual-networks-vnet"></a>Sanal ağlardan (VNet) erişimi yapılandırma

Azure Cosmos DB hesaplarını, yalnızca Azure sanal ağının belirli bir alt ağından erişime izin verecek şekilde yapılandırabilirsiniz. Bir Azure Cosmos DB hesabına erişimi, bir sanal ağdaki bir alt ağdan bağlantılarla sınırlamak için:

1. Alt ağ ve sanal ağ kimliğini Azure Cosmos DB göndermek için alt ağı etkinleştirin. Bu, belirli bir alt ağdaki Azure Cosmos DB için bir hizmet uç noktası etkinleştirerek elde edebilirsiniz.

1. Alt ağı hesabın erişebileceği bir kaynak olarak belirtmek için Azure Cosmos DB hesaba bir kural ekleyin.

> [!NOTE]
> Bir alt ağda Azure Cosmos DB hesabınız için bir hizmet uç noktası etkinleştirildiğinde, bir genel IP 'den bir sanal ağa ve alt ağa Azure Cosmos DB anahtarlara ulaşan trafik kaynağıdır. Trafik geçişi, bu alt ağdan erişilen tüm Azure Cosmos DB hesapları için geçerlidir. Azure Cosmos DB hesaplarınızın bu alt ağa izin veren IP tabanlı bir güvenlik duvarı varsa, hizmet etkin alt ağdan gelen istekler artık IP güvenlik duvarı kurallarıyla eşleşmez ve reddedilir.
>
> Daha fazla bilgi edinmek için, bu makalenin [IP güvenlik duvarı kuralından bir sanal ağ erişim denetim listesine geçme](#migrate-from-firewall-to-vnet) bölümünde özetlenen adımlara bakın.

Aşağıdaki bölümlerde, bir Azure Cosmos DB hesabı için sanal ağ hizmeti uç noktasının nasıl yapılandırılacağı açıklanır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configure-a-service-endpoint-by-using-the-azure-portal"></a><a id="configure-using-portal"></a>Azure portal kullanarak bir hizmet uç noktası yapılandırma

### <a name="configure-a-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Mevcut bir Azure sanal ağı ve alt ağı için bir hizmet uç noktası yapılandırın

1. **Tüm kaynaklar** dikey penceresinde, güvenli hale getirmek istediğiniz Azure Cosmos DB hesabını bulun.

1. Ayarlar menüsünden **güvenlik duvarları ve sanal ağlar** ' ı seçin ve **Seçilen ağlardan**erişime izin ver ' i seçin.

1. Var olan bir sanal ağın alt ağına erişim vermek için, **sanal ağlar**altında **var olan Azure sanal ağını Ekle**' yi seçin.

1. Azure sanal ağını eklemek istediğiniz **aboneliği** seçin. Azure Cosmos DB hesabınıza erişim sağlamak istediğiniz Azure **sanal ağlarını** ve **alt ağlarını** seçin. Sonra, "Microsoft. Azu, Smosdb" için hizmet uç noktalarına sahip seçili ağları etkinleştirmek üzere **Etkinleştir** ' i seçin. Tamamlandıktan sonra **Ekle**' yi seçin.

   ![Sanal ağ ve alt ağ seçin](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png)

1. Azure Cosmos DB hesabı bir sanal ağdan erişim için etkinleştirildikten sonra, yalnızca bu seçili alt ağdan gelen trafiğe izin verir. Eklediğiniz sanal ağ ve alt ağ aşağıdaki ekran görüntüsünde gösterildiği gibi görünmelidir:

   ![Sanal ağ ve alt ağ başarıyla yapılandırıldı](./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

> [!NOTE]
> Sanal ağ hizmet uç noktalarını etkinleştirmek için aşağıdaki Abonelik izinlerine sahip olmanız gerekir:
>   * Sanal ağı olan abonelik: ağ katılımcısı
>   * Azure Cosmos DB hesabıyla abonelik: DocumentDB hesabı Katılımcısı
>   * Sanal ağınız ve Azure Cosmos DB hesabınız farklı aboneliklerdeyse, sanal ağa sahip aboneliğin de `Microsoft.DocumentDB` kaynak sağlayıcısı kayıtlı olduğundan emin olun. Bir kaynak sağlayıcısını kaydetmek için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md) makalesi.

Aboneliği kaynak sağlayıcısına kaydetme yönergeleri aşağıda verilmiştir.

### <a name="configure-a-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Yeni bir Azure sanal ağı ve alt ağı için hizmet uç noktası yapılandırma

1. **Tüm kaynaklar** dikey penceresinde, güvenli hale getirmek istediğiniz Azure Cosmos DB hesabını bulun.  

1. Ayarlar menüsünden **güvenlik duvarları ve Azure sanal ağları** ' nı seçin ve **Seçilen ağlardan**erişime izin ver ' i seçin.  

1. Yeni bir Azure sanal ağına erişim vermek için, **sanal ağlar**altında **Yeni sanal ağ ekle**' yi seçin.  

1. Yeni bir sanal ağ oluşturmak için gereken ayrıntıları girip **Oluştur**' u seçin. Alt ağ "Microsoft. Azu, Smosdb" etkin bir hizmet uç noktası ile oluşturulur.

   ![Yeni bir sanal ağ için bir sanal ağ ve alt ağ seçin](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

Azure Cosmos DB hesabınız Azure Bilişsel Arama gibi diğer Azure hizmetleri tarafından kullanılıyorsa veya Stream Analytics veya Power BI 'e erişildiğinde, **Genel Azure veri merkezlerinden gelen bağlantıları kabul et**' i seçerek erişime izin verebilirsiniz.

Portaldan Azure Cosmos DB ölçümlerine erişiminizin olduğundan emin olmak için **Azure Portal seçeneklerinden erişime Izin ver** ' i etkinleştirmeniz gerekir. Bu seçenekler hakkında daha fazla bilgi edinmek için bkz. [IP güvenlik duvarı yapılandırma](how-to-configure-firewall.md) makalesi. Erişimi etkinleştirdikten sonra ayarları kaydetmek için **Kaydet** ' i seçin.

## <a name="remove-a-virtual-network-or-subnet"></a><a id="remove-vnet-or-subnet"></a>Sanal ağ veya alt ağ kaldırma

1. **Tüm kaynaklar** dikey penceresinde, hizmet uç noktalarını atadığınız Azure Cosmos DB hesabını bulun.  

1. Ayarlar menüsünden **güvenlik duvarları ve sanal ağlar ' ı** seçin.  

1. Bir sanal ağ veya alt ağ kuralını kaldırmak için, sanal ağ veya alt ağın yanındaki **...** öğesini seçin ve **Kaldır**' ı seçin.

   ![Sanal ağı kaldır](./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png)

1. Değişikliklerinizi uygulamak için **Kaydet**’i seçin.

## <a name="configure-a-service-endpoint-by-using-azure-powershell"></a><a id="configure-using-powershell"></a>Azure PowerShell kullanarak bir hizmet uç noktası yapılandırma

> [!NOTE]
> PowerShell veya Azure CLı kullanırken, yalnızca eklenmesi gereken tek değil, parametrelerde IP filtrelerinin ve sanal ağ ACL 'lerinin tam listesini belirttiğinizden emin olun.

Azure PowerShell kullanarak bir hizmet uç noktasını bir Azure Cosmos DB hesabına yapılandırmak için aşağıdaki adımları kullanın:  

1. [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps) yükleyip [oturum açın](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  

1. Bir sanal ağın mevcut alt ağı için hizmet uç noktasını etkinleştirin.  

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

1. Sanal ağ bilgilerini al.

   ```powershell
   $vnet = Get-AzVirtualNetwork `
      -ResourceGroupName $resourceGroupName `
      -Name $vnetName

   $subnetId = $vnet.Id + "/subnets/" + $subnetName
   ```

1. Cosmos DB sanal ağ kuralı hazırlama

   ```powershell
   $vnetRule = New-AzCosmosDBVirtualNetworkRule `
      -Id $subnetId
   ```

1. Yeni sanal ağ uç noktası yapılandırmasıyla Azure Cosmos DB hesabı özelliklerini güncelleştirin: 

   ```powershell
   $accountName = "<Cosmos DB account name>"

   Update-AzCosmosDBAccount `
      -ResourceGroupName $resourceGroupName `
      -Name $accountName `
      -EnableVirtualNetwork $true `
      -VirtualNetworkRuleObject @($vnetRule)
   ```

1. Azure Cosmos DB hesabınızın, önceki adımda yapılandırdığınız sanal ağ hizmeti uç noktasıyla güncelleştirildiğinden emin olmak için aşağıdaki komutu çalıştırın:

   ```powershell
   $account = Get-AzCosmosDBAccount `
      -ResourceGroupName $resourceGroupName `
      -Name $accountName

   $account.IsVirtualNetworkFilterEnabled
   $account.VirtualNetworkRules
   ```

## <a name="configure-a-service-endpoint-by-using-the-azure-cli"></a><a id="configure-using-cli"></a>Azure CLı kullanarak hizmet uç noktası yapılandırma

Azure Cosmos hesapları, alt ağ zaten yapılandırılmışsa, daha sonra oluşturulduklarında veya güncellendiklerinde hizmet uç noktaları için yapılandırılabilir. Hizmet uç noktaları, alt ağın henüz kendileri için yapılandırılmadığı ve sonra alt ağ daha sonra yapılandırıldığında çalışmaya başlayacağı Cosmos hesabında da etkinleştirilebilir. Bu esneklik, yapılandırmalarının birbirini birbirinden bağımsız hale getirmek için hem Cosmos hesabına hem de sanal ağ kaynaklarına erişimi olmayan yöneticilere izin verir.

### <a name="create-a-new-cosmos-account-and-connect-it-to-a-back-end-subnet-for-a-new-virtual-network"></a>Yeni bir Cosmos hesabı oluşturun ve yeni bir sanal ağ için bir arka uç alt ağına bağlayın

Bu örnekte, sanal ağ ve alt ağ, her ikisi de oluşturulduğunda etkinleştirilmiş hizmet uç noktaları ile oluşturulur.

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

### <a name="connect-and-configure-a-cosmos-account-to-a-back-end-subnet-independently"></a>Cosmos hesabını bağımsız olarak arka uç alt ağına bağlama ve yapılandırma

Bu örnek, bir Azure Cosmos hesabının alt ağın henüz hizmet uç noktaları için yapılandırılmadığı mevcut yeni bir sanal ağa nasıl bağlanacağını göstermek için tasarlanmıştır. Bu, `--ignore-missing-vnet-service-endpoint` parametresi kullanılarak yapılır. Bu, sanal ağın alt ağının yapılandırması tamamlanmadan önce Cosmos hesabının yapılandırmasının hatasız tamamlanmasını sağlar. Alt ağ yapılandırması tamamlandıktan sonra, Cosmos hesabına, yapılandırılmış alt ağ üzerinden erişilebilecektir.

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

## <a name="migrating-from-an-ip-firewall-rule-to-a-virtual-network-acl"></a><a id="migrate-from-firewall-to-vnet"></a>IP güvenlik duvarı kuralından bir sanal ağ ACL 'sine geçiş

Sanal ağ hizmet uç noktalarını kullanmak için IP güvenlik duvarı kurallarını kullanarak bir Azure Cosmos DB hesabını geçirmek için aşağıdaki adımları kullanın.

Bir alt ağ için hizmet uç noktası için bir Azure Cosmos DB hesabı yapılandırıldıktan sonra, bu alt ağdan gelen istekler, kaynak genel IP adresi yerine sanal ağ ve alt ağ kaynak bilgileriyle Azure Cosmos DB gönderilir. Bu istekler artık Azure Cosmos DB hesabında yapılandırılmış bir IP filtresiyle eşleşmez, bu nedenle kapalı kalma süresini önlemek için aşağıdaki adımların gerekli olmasının nedeni budur.

Devam etmeden önce, sanal ağ ve alt ağ üzerindeki Azure Cosmos DB hizmet uç noktasını, "sanal ağın mevcut alt ağı için hizmet uç noktasını etkinleştir" bölümünde gösterilen adımı kullanarak etkinleştirin.

1. Sanal ağ ve alt ağ bilgilerini al:

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

1. Azure Cosmos DB hesabı için yeni bir sanal ağ kuralı nesnesi hazırlayın:

   ```powershell
   $vnetRule = New-AzCosmosDBVirtualNetworkRule `
      -Id $subnetId
   ```

1. Alt ağdan hizmet uç noktası erişimini etkinleştirmek için Azure Cosmos DB hesabını güncelleştirin:

   ```powershell
   Update-AzCosmosDBAccount `
      -ResourceGroupName $resourceGroupName `
      -Name $accountName `
      -EnableVirtualNetwork $true `
      -VirtualNetworkRuleObject @($vnetRule)
   ```

1. Alt ağdan erişilen tüm Azure Cosmos DB hesapları için önceki adımları tekrarlayın.

1. Alt ağ için IP güvenlik duvarı kuralını Azure Cosmos DB hesabının güvenlik duvarı kurallarından kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB için bir güvenlik duvarı yapılandırmak için, [güvenlik duvarı destek](firewall-support.md) makalesine bakın.
