---
title: Azure Cosmos DB hesabınız için bir IP güvenlik duvarı yapılandırın
description: Azure Cosmos hesaplarında güvenlik duvarı desteği için IP erişim denetim ilkelerini yapılandırmayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/24/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: 69c39d2478ed7d488c1209c2c7e16c241c59bcef
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88814187"
---
# <a name="configure-ip-firewall-in-azure-cosmos-db"></a>Azure Cosmos DB 'de IP güvenlik duvarını yapılandırma

IP güvenlik duvarlarını kullanarak Azure Cosmos DB hesabınızda depolanan verilerin güvenliğini sağlayabilirsiniz. Azure Cosmos DB, gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Aşağıdaki yollarla Azure Cosmos DB hesapta bir IP güvenlik duvarı belirleyebilirsiniz:

* Azure portalından
* Azure Resource Manager şablonu kullanarak bildirimli olarak
* **İpRangeFilter** özelliğini GÜNCELLEŞTIREREK Azure clı veya Azure PowerShell aracılığıyla programlama yoluyla

## <a name="configure-an-ip-firewall-by-using-the-azure-portal"></a><a id="configure-ip-policy"></a> Azure portal kullanarak bir IP güvenlik duvarı yapılandırma

Azure portal IP erişim denetimi ilkesini ayarlamak için, Azure Cosmos DB hesabı sayfasına gidin ve gezinti menüsünde **güvenlik duvarı ve sanal ağlar** ' ı seçin. Değerden **erişime Izin ver** ' i **Seçili ağlara**değiştirin ve ardından **Kaydet**' i seçin.

:::image type="content" source="./media/how-to-configure-firewall/azure-portal-firewall.png" alt-text="Azure portal güvenlik duvarı sayfasının nasıl açılacağını gösteren ekran görüntüsü":::

IP erişim denetimi açıldığında, Azure portal IP adresleri, IP adresi aralıkları ve anahtarlar belirtme olanağı sağlar. Anahtarlar, diğer Azure hizmetlerine ve Azure portal erişimi etkinleştirir. Aşağıdaki bölümlerde bu anahtarlarla ilgili ayrıntılar verilmektedir.

> [!NOTE]
> Azure Cosmos DB hesabınız için bir IP erişim denetimi ilkesini etkinleştirdikten sonra, izin verilen IP adresi aralıkları dışındaki makinelerden gelen Azure Cosmos DB hesabınıza yapılan tüm istekler reddedilir. Access Control 'un bütünlüğünü sağlamak için portaldan Azure Cosmos DB kaynaklarına göz atma de engellenir.

### <a name="allow-requests-from-the-azure-portal"></a>Azure portal isteklere izin ver

Bir IP erişim denetimi ilkesini programlı bir şekilde etkinleştirdiğinizde, erişimi sürdürmek için Azure portal IP adresini **ipRangeFilter** özelliğine eklemeniz gerekir. Portal IP adresleri şunlardır:

|Region|IP adresi|
|------|----------|
|Almanya|51.4.229.218|
|Çin|139.217.8.252|
|US Gov|52.244.48.71|
|Diğer tüm bölgeler|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Aşağıdaki ekran görüntüsünde gösterildiği gibi, **Azure Portal erişime Izin ver** seçeneğini belirleyerek Azure Portal erişim isteklerini etkinleştirebilirsiniz:

:::image type="content" source="./media/how-to-configure-firewall/enable-azure-portal.png" alt-text="Azure portal erişimin nasıl etkinleştirileceğini gösteren ekran görüntüsü":::

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Azure içinde küresel Azure veri merkezlerinden veya başka kaynaklardan gelen isteklere izin verme

Azure Cosmos DB hesabınıza statik IP (örneğin, Azure Stream Analytics ve Azure Işlevleri) sağlamayan hizmetlerden eriştiğinizde, erişimi kısıtlamak için IP güvenlik duvarını kullanmaya devam edebilirsiniz. Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure **veri merkezleri içinden bağlantıları kabul et** seçeneğini belirleyerek Azure 'daki diğer kaynaklardan erişimi etkinleştirebilirsiniz:

:::image type="content" source="./media/how-to-configure-firewall/enable-azure-services.png" alt-text="Azure veri merkezlerinden bağlantıların nasıl kabul edileceği gösteren ekran görüntüsü":::

Bu seçeneği etkinleştirdiğinizde, IP adresi `0.0.0.0` izin VERILEN IP adresleri listesine eklenir. `0.0.0.0`IP adresi, Istekleri Azure veri MERKEZI IP aralığından Azure Cosmos DB hesabınıza kısıtlar. Bu ayar Azure Cosmos DB hesabınıza başka hiçbir IP aralığından erişime izin vermez.

> [!NOTE]
> Bu seçenek, Azure 'da dağıtılan diğer müşterilerin aboneliklerinden gelen istekler dahil olmak üzere güvenlik duvarını Azure 'daki tüm isteklere izin verecek şekilde yapılandırır. Bu seçenek tarafından izin verilen IP 'lerin listesi geniştir, bu nedenle bir güvenlik duvarı ilkesinin verimliliğini kısıtlar. Bu seçeneği yalnızca istekleriniz sanal ağlardaki statik IP 'lerden veya alt ağlardan kaynaklanmıyorsanız kullanın. Azure portal Azure 'da dağıtıldığından, bu seçeneğin belirlenmesi otomatik olarak Azure portal erişimine izin verir.

### <a name="requests-from-your-current-ip"></a>Geçerli IP 'nizden gelen istekler

Azure portal geliştirmeyi basitleştirmek için, istemci makinenizin IP 'sini belirlemenize ve izin verilenler listesine eklemenize yardımcı olur. Makinenizi çalıştıran uygulamalar daha sonra Azure Cosmos DB hesabınıza erişebilir.

Portal, istemci IP adresini otomatik olarak algılar. Makinenizin istemci IP adresi veya ağ geçidinizin IP adresi olabilir. İş yüklerinizi üretime almadan önce bu IP adresini kaldırdığınızdan emin olun.

Geçerli IP 'nizi IP listesine eklemek için **GEÇERLI IP 'Yi Ekle**' yi seçin. Sonra **Kaydet**'i seçin.

:::image type="content" source="./media/how-to-configure-firewall/enable-current-ip.png" alt-text="Geçerli IP için güvenlik duvarı ayarlarının nasıl yapılandırılacağını gösteren ekran görüntüsü":::

### <a name="requests-from-cloud-services"></a>Bulut hizmetlerinden gelen istekler

Azure 'da bulut Hizmetleri, Azure Cosmos DB kullanarak orta katman hizmet mantığını barındırmak için ortak bir yoldur. Bulut hizmetinden Azure Cosmos DB hesabınıza erişimi etkinleştirmek için, [IP erişim denetimi ilkesini yapılandırarak](#configure-ip-policy), bulut HIZMETININ genel ıp adresini Azure Cosmos DB hesabınızla ilişkilendirilen ızın verilen IP adresleri listesine eklemeniz gerekir. Bu, bulut hizmetlerinin tüm rol örneklerinin Azure Cosmos DB hesabınıza erişmesini sağlar.

Aşağıdaki ekran görüntüsünde gösterildiği gibi, Azure portal bulut hizmetlerinizin IP adreslerini alabilirsiniz:

:::image type="content" source="./media/how-to-configure-firewall/public-ip-addresses.png" alt-text="Azure portal gösterilen bir bulut hizmeti için genel IP adresini gösteren ekran görüntüsü":::

Rol örnekleri ekleyerek bulut hizmetinizi ölçeklendirirseniz, aynı bulut hizmetinin bir parçası olduklarından, bu yeni örnekler otomatik olarak Azure Cosmos DB hesaba erişebilir.

### <a name="requests-from-virtual-machines"></a>Sanal makinelerden gelen istekler

Azure Cosmos DB kullanarak orta katmanlı Hizmetleri barındırmak için [sanal makineleri](https://azure.microsoft.com/services/virtual-machines/) veya [sanal makine ölçek kümelerini](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) de kullanabilirsiniz. Cosmos DB hesabınızı sanal makinelerden erişime izin verecek şekilde yapılandırmak için, [IP erişim denetimi ilkesini yapılandırarak](#configure-ip-policy)Azure Cosmos DB hesabınız için ızın verilen IP adreslerinden biri olarak sanal makinenin ve/veya sanal makine ölçek KÜMESININ genel IP adresini yapılandırmanız gerekir.

Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portal sanal makinelerin IP adreslerini alabilirsiniz:

:::image type="content" source="./media/how-to-configure-firewall/public-ip-addresses-dns.png" alt-text="Azure portal gösterilen bir sanal makinenin genel IP adresini gösteren ekran görüntüsü":::

Gruba sanal makine örnekleri eklediğinizde Azure Cosmos DB hesabınıza otomatik olarak erişim elde edersiniz.

### <a name="requests-from-the-internet"></a>İnternet 'ten gelen istekler

Azure Cosmos DB hesabınıza Internet 'teki bir bilgisayardan eriştiğinizde, hesabın istemci IP adresi veya IP adresi aralığı, hesabınız için izin verilen IP adresleri listesine eklenmelidir.

## <a name="configure-an-ip-firewall-by-using-a-resource-manager-template"></a><a id="configure-ip-firewall-arm"></a>Kaynak Yöneticisi şablonu kullanarak bir IP güvenlik duvarı yapılandırma

Azure Cosmos DB hesabınıza erişim denetimini yapılandırmak için Kaynak Yöneticisi şablonunun, izin verilen IP aralıkları dizisiyle **ıprules** özelliğini belirttiğinden emin olun. IP güvenlik duvarını zaten dağıtılmış bir Cosmos hesabına yapılandırıyorsanız, `locations` dizinin şu anda dağıtılmış olan ile eşleştiğinden emin olun. `locations`Diziyi ve diğer özellikleri eşzamanlı olarak değiştiremezsiniz. Azure Cosmos DB için Azure Resource Manager şablonlarının daha fazla bilgi ve örnekleri için bkz. [Azure Resource Manager şablonları Azure Cosmos DB](resource-manager-samples.md)

> [!IMPORTANT]
> **Iprules** özelliği apı sürümü 2020-04-01 ile tanıtılmıştır. Önceki sürümler bunun yerine, virgülle ayrılmış IP adresleri listesi olan bir **ipRangeFilter** özelliği kullanıma sunuldu.

Aşağıdaki örnekte, **ıprules** özelliğinin apı sürüm 2020-04-01 veya sonraki bir sürümünde nasıl açığa çıkarılagösterdiği gösterilmektedir:

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2020-04-01",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "enableAutomaticFailover": "[parameters('automaticFailover')]",
    "ipRules": [
      {
        "ipAddressOrRange": "40.76.54.131"
      },
      {
        "ipAddressOrRange": "52.176.6.30"
      },
      {
        "ipAddressOrRange": "52.169.50.45"
      },
      {
        "ipAddressOrRange": "52.187.184.26"
      }
    ]
  }
}
```

2020-04-01 ' dan önceki API sürümleri için aynı örnek aşağıda verilmiştir:

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2019-08-01",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "enableAutomaticFailover": "[parameters('automaticFailover')]",
    "ipRangeFilter":"40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
  }
}
```

## <a name="configure-an-ip-access-control-policy-by-using-the-azure-cli"></a><a id="configure-ip-firewall-cli"></a>Azure CLı kullanarak bir IP erişim denetim ilkesi yapılandırma

Aşağıdaki komut, IP erişim denetimi ile bir Azure Cosmos DB hesabının nasıl oluşturulacağını gösterir:

```azurecli-interactive
# Create a Cosmos DB account with default values and IP Firewall enabled
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
ipRangeFilter='192.168.221.17,183.240.196.255,40.76.54.131'

# Make sure there are no spaces in the comma-delimited list of IP addresses or CIDR ranges.
az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --locations regionName='West US 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='East US 2' failoverPriority=1 isZoneRedundant=False \
    --ip-range-filter $ipRangeFilter
```

## <a name="configure-an-ip-access-control-policy-by-using-powershell"></a><a id="configure-ip-firewall-ps"></a>PowerShell kullanarak bir IP erişim denetim ilkesi yapılandırma

Aşağıdaki betik, IP erişim denetimi ile bir Azure Cosmos DB hesabının nasıl oluşturulacağını gösterir:

```azurepowershell-interactive
# Create a Cosmos DB account with default values and IP Firewall enabled
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$ipRules = @("192.168.221.17","183.240.196.255","40.76.54.131")

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0; "isZoneRedundant"=False },
    @{ "locationName"="East US 2"; "failoverPriority"=1, "isZoneRedundant"=False }
)

# Make sure there are no spaces in the comma-delimited list of IP addresses or CIDR ranges.
$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "ipRules"=$ipRules
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2020-04-01" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="troubleshoot-issues-with-an-ip-access-control-policy"></a><a id="troubleshoot-ip-firewall"></a>IP erişim denetimi ilkesiyle ilgili sorunları giderme

Aşağıdaki seçenekleri kullanarak bir IP erişim denetimi ilkesiyle ilgili sorunları çözebilirsiniz:

### <a name="azure-portal"></a>Azure portal

Azure Cosmos DB hesabınız için bir IP erişim denetimi ilkesi etkinleştirerek, hesabınıza verilen IP adresi aralıkları listesinin dışındaki tüm istekleri engellenir. Kapsayıcılara göz atma ve belge sorgulama gibi portal veri düzlemi işlemlerini etkinleştirmek için, portalda **güvenlik duvarı** bölmesini kullanarak Azure Portal erişime açıkça izin vermeniz gerekir.

### <a name="sdks"></a>SDK

İzin verilenler listesinde olmayan makinelerden SDK 'Ları kullanarak Azure Cosmos DB kaynaklarına eriştiğinizde, ek ayrıntı olmadan genel **403 yasaklanmış** bir yanıt döndürülür. Hesabınız için izin verilen IP listesini doğrulayın ve Azure Cosmos DB hesabınıza doğru ilke yapılandırmasının uygulandığından emin olun.

### <a name="source-ips-in-blocked-requests"></a>Engellenen isteklerde kaynak IP 'Leri

Azure Cosmos DB hesabınızda tanılama günlüğünü etkinleştirin. Bu Günlükler her isteği ve yanıtı gösterir. Güvenlik Duvarı ile ilgili iletiler 403 dönüş koduyla günlüğe kaydedilir. Bu iletileri filtreleyerek, engellenen isteklerin kaynak IP 'lerini görebilirsiniz. Bkz. [Azure Cosmos DB tanılama günlüğü](logging.md).

### <a name="requests-from-a-subnet-with-a-service-endpoint-for-azure-cosmos-db-enabled"></a>Azure Cosmos DB etkin için bir hizmet uç noktası olan bir alt ağdan gelen istekler

Azure Cosmos DB etkin için bir hizmet uç noktası olan bir sanal ağ alt ağından gelen istekler, sanal ağ ve alt ağ kimliğini Azure Cosmos DB hesaplara gönderir. Bu istekler, kaynağın genel IP 'si içermez, bu nedenle IP filtreleri bunları reddeder. Sanal ağlardaki belirli alt ağlardan erişime izin vermek için, [Azure Cosmos DB hesabınız için sanal ağ ve alt ağ tabanlı erişimin nasıl yapılandırılacağı konusunda](how-to-configure-vnet-service-endpoint.md)özetlenen bir erişim denetim listesi ekleyin. Güvenlik Duvarı kurallarının uygulanması 15 dakika kadar sürebilir.

### <a name="private-ip-addresses-in-list-of-allowed-addresses"></a>İzin verilen adresler listesindeki özel IP adresleri

Bir Azure Cosmos hesabını özel IP adresleri içeren bir izin verilen adresler listesiyle oluşturmak veya güncelleştirmek başarısız olur. Listede özel IP adresi belirtilmediğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB hesabınız için bir sanal ağ hizmeti uç noktası yapılandırmak için aşağıdaki makalelere bakın:

* [Azure Cosmos DB hesabınız için sanal ağ ve alt ağ erişim denetimi](vnet-service-endpoint.md)
* [Azure Cosmos DB hesabınız için sanal ağ ve alt ağ tabanlı erişimi yapılandırın](how-to-configure-vnet-service-endpoint.md)
