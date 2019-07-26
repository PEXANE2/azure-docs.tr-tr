---
title: Azure Cosmos DB hesabınız için bir IP güvenlik duvarı yapılandırın
description: Azure Cosmos DB veritabanı hesaplarında güvenlik duvarı desteği için IP erişim denetim ilkelerini yapılandırmayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/25/2019
ms.author: mjbrown
ms.openlocfilehash: 0b8ad6c5addbff293e9f7e9b8af6ed34d4dd274b
ms.sourcegitcommit: 5604661655840c428045eb837fb8704dca811da0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68494887"
---
# <a name="configure-ip-firewall-in-azure-cosmos-db"></a>Azure Cosmos DB 'de IP güvenlik duvarını yapılandırma

IP güvenlik duvarlarını kullanarak Azure Cosmos DB hesabınızda depolanan verilerin güvenliğini sağlayabilirsiniz. Azure Cosmos DB, gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Aşağıdaki yollarla Azure Cosmos DB hesapta bir IP güvenlik duvarı belirleyebilirsiniz:

* Azure portal
* Bildirimli bir Azure Resource Manager şablonu kullanarak
* **İpRangeFilter** özelliğini GÜNCELLEŞTIREREK Azure clı veya Azure PowerShell aracılığıyla programlama yoluyla

## <a id="configure-ip-policy"></a>Azure portal kullanarak bir IP güvenlik duvarı yapılandırma

Azure portal IP erişim denetimi ilkesini ayarlamak için, Azure Cosmos DB hesabı sayfasına gidin ve gezinti menüsünde **güvenlik duvarı ve sanal ağlar** ' ı seçin. Değerden **erişime Izin ver** ' i **Seçili ağlara**değiştirin ve ardından **Kaydet**' i seçin. 

![Azure portal güvenlik duvarı sayfasının nasıl açılacağını gösteren ekran görüntüsü](./media/how-to-configure-firewall/azure-portal-firewall.png)

IP erişim denetimi açıldığında, Azure portal IP adresleri, IP adresi aralıkları ve anahtarlar belirtme olanağı sağlar. Anahtarlar, diğer Azure hizmetlerine ve Azure portal erişimi etkinleştirir. Aşağıdaki bölümlerde bu anahtarlarla ilgili ayrıntılar verilmektedir.

> [!NOTE]
> Azure Cosmos DB hesabınız için bir IP erişim denetimi ilkesini etkinleştirdikten sonra, izin verilen IP adresi aralıkları dışındaki makinelerden gelen Azure Cosmos DB hesabınıza yapılan tüm istekler reddedilir. Access Control 'un bütünlüğünü sağlamak için portaldan Azure Cosmos DB kaynaklarına göz atma de engellenir.

### <a name="allow-requests-from-the-azure-portal"></a>Azure portal isteklere izin ver

Bir IP erişim denetimi ilkesini programlı bir şekilde etkinleştirdiğinizde, erişimi sürdürmek için Azure portal IP adresini **ipRangeFilter** özelliğine eklemeniz gerekir. Portal IP adresleri şunlardır:

|Bölge|IP adresi|
|------|----------|
|Almanya|51.4.229.218|
|Çin|139.217.8.252|
|US Gov|52.244.48.71|
|Diğer tüm bölgeler|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Aşağıdaki ekran görüntüsünde gösterildiği gibi, **Azure Portal erişime Izin ver** seçeneğini belirleyerek Azure Portal erişimi etkinleştirebilirsiniz: 

![Azure portal erişimin nasıl etkinleştirileceğini gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Küresel Azure veri merkezlerinden veya Azure 'daki diğer kaynaklardan gelen isteklere izin ver

Azure Cosmos DB hesabınıza statik IP (örneğin, Azure Stream Analytics ve Azure Işlevleri) sağlamayan hizmetlerden eriştiğinizde, erişimi kısıtlamak için IP güvenlik duvarını kullanmaya devam edebilirsiniz. Bu türden hizmetlerden Azure Cosmos DB hesabına erişime izin vermek için 0.0.0.0 IP adresini izin verilen IP adresleri listesine ekleyin. 0\.0.0.0 adresi, istekleri Azure veri merkezi IP aralığından Azure Cosmos DB hesabınıza kısıtlar. Bu ayar Azure Cosmos DB hesabınıza başka herhangi bir IP aralığı için erişime izin vermez.

> [!NOTE]
> Bu seçenek, Azure 'da dağıtılan diğer müşterilerin aboneliklerinden gelen istekler dahil olmak üzere güvenlik duvarını Azure 'daki tüm isteklere izin verecek şekilde yapılandırır. Bu seçenek tarafından izin verilen IP 'lerin listesi geniştir, bu nedenle bir güvenlik duvarı ilkesinin verimliliğini kısıtlar. Bu seçeneği yalnızca istekleriniz sanal ağlardaki statik IP 'lerden veya alt ağlardan kaynaklanmıyorsanız kullanın. Azure portal Azure 'da dağıtıldığından, bu seçeneğin belirlenmesi otomatik olarak Azure portal erişimine izin verir.

Aşağıdaki ekran görüntüsünde gösterildiği gibi, **Genel Azure veri merkezleri içinden bağlantıları kabul et** seçeneğini belirleyerek Azure Portal erişimi etkinleştirebilirsiniz: 

![Azure portal güvenlik duvarı sayfasının nasıl açılacağını gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Geçerli IP 'nizden gelen istekler

Azure portal geliştirmeyi basitleştirmek için, istemci makinenizin IP 'sini belirlemenize ve izin verilenler listesine eklemenize yardımcı olur. Makinenizi çalıştıran uygulamalar daha sonra Azure Cosmos DB hesabınıza erişebilir. 

Portal, istemci IP adresini otomatik olarak algılar. Makinenizin istemci IP adresi veya ağ geçidinizin IP adresi olabilir. İş yüklerinizi üretime almadan önce bu IP adresini kaldırdığınızdan emin olun. 

Geçerli IP 'nizi IP listesine eklemek için **GEÇERLI IP 'Yi Ekle**' yi seçin. Daha sonra **Kaydet**’e tıklayın.

![Geçerli IP için güvenlik duvarı ayarlarının nasıl yapılandırılacağını gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Bulut hizmetlerinden gelen istekler

Azure 'da bulut Hizmetleri, Azure Cosmos DB kullanarak orta katman hizmet mantığını barındırmak için ortak bir yoldur. Bulut hizmetinden Azure Cosmos DB hesabınıza erişimi etkinleştirmek için, [IP erişim denetimi ilkesini yapılandırarak](#configure-ip-policy), bulut HIZMETININ genel ıp adresini Azure Cosmos DB hesabınızla ilişkilendirilen ızın verilen IP adresleri listesine eklemeniz gerekir. Bu, bulut hizmetlerinin tüm rol örneklerinin Azure Cosmos DB hesabınıza erişmesini sağlar. 

Aşağıdaki ekran görüntüsünde gösterildiği gibi, Azure portal bulut hizmetlerinizin IP adreslerini alabilirsiniz:

![Azure portal gösterilen bir bulut hizmeti için genel IP adresini gösteren ekran görüntüsü](./media/how-to-configure-firewall/public-ip-addresses.png)

Rol örnekleri ekleyerek bulut hizmetinizi ölçeklendirirseniz, aynı bulut hizmetinin bir parçası olduklarından, bu yeni örnekler otomatik olarak Azure Cosmos DB hesaba erişebilir.

### <a name="requests-from-virtual-machines"></a>Sanal makinelerden gelen istekler

Azure Cosmos DB kullanarak orta katmanlı Hizmetleri barındırmak için [sanal makineleri](https://azure.microsoft.com/services/virtual-machines/) veya [sanal makine ölçek kümelerini](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) de kullanabilirsiniz. Cosmos DB hesabınızı sanal makinelerden erişime izin verecek şekilde yapılandırmak için, sanal makine ve/veya sanal makine ölçek kümesinin genel IP adresini Azure Cosmos DB hesabınız için izin verilen IP adreslerinden biri olarak yapılandırmanız gerekir. [ IP erişim denetim ilkesi](#configure-ip-policy). 

Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portal sanal makinelerin IP adreslerini alabilirsiniz:

![Azure portal gösterilen bir sanal makinenin genel IP adresini gösteren ekran görüntüsü](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Gruba sanal makine örnekleri eklediğinizde Azure Cosmos DB hesabınıza otomatik olarak erişim elde edersiniz.

### <a name="requests-from-the-internet"></a>İnternet 'ten gelen istekler

Azure Cosmos DB hesabınıza Internet 'teki bir bilgisayardan eriştiğinizde, hesabın istemci IP adresi veya IP adresi aralığı, hesabınız için izin verilen IP adresleri listesine eklenmelidir.

## <a id="configure-ip-firewall-arm"></a>Kaynak Yöneticisi şablonu kullanarak bir IP güvenlik duvarı yapılandırma

Azure Cosmos DB hesabınıza erişim denetimini yapılandırmak için, Kaynak Yöneticisi şablonunun izin verilen IP aralıkları listesi ile **ipRangeFilter** özniteliğini belirttiğinden emin olun. IP güvenlik duvarını zaten dağıtılmış bir Cosmos hesabına yapılandırıyorsanız, `locations` dizinin şu anda dağıtılmış olan ile eşleştiğinden emin olun. `locations` Diziyi ve diğer özellikleri eşzamanlı olarak değiştiremezsiniz. Azure Cosmos DB için ARM şablonlarının daha fazla bilgi ve örnekleri için bkz. [Azure Cosmos DB için Azure Resource Manager şablonları](resource-manager-samples.md)

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2016-03-31",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "enableAutomaticFailover": "[parameters('automaticFailover')]",
    "enableMultipleWriteLocations": "[parameters('multipleWriteLocations')]",
    "ipRangeFilter":"183.240.196.255,104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
  }
}
```

## <a id="configure-ip-firewall-cli"></a>Azure CLı kullanarak bir IP erişim denetim ilkesi yapılandırma

Aşağıdaki komut, IP erişim denetimi ile bir Azure Cosmos DB hesabının nasıl oluşturulacağını gösterir: 

```azurecli-interactive

name="<Azure Cosmos DB account name>"
resourceGroupName="<Resource group name>"

az cosmosdb create \
  --name $name \
  --kind GlobalDocumentDB \
  --resource-group $resourceGroupName \
  --max-interval 10 \
  --max-staleness-prefix 200 \
  --ip-range-filter "183.240.196.255,104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
```

Mevcut bir hesabın güvenlik duvarı ayarlarını güncelleştirmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb update \
      --name $name \
      --resource-group $resourceGroupName \
      --ip-range-filter "183.240.196.255,104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
```

## <a id="configure-ip-firewall-ps"></a>PowerShell kullanarak bir IP erişim denetim ilkesi yapılandırma

Aşağıdaki betik, IP erişim denetimi ile bir Azure Cosmos DB hesabının nasıl oluşturulacağını gösterir:

```azurepowershell-interactive

$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$locations = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)

# Add local machine's IP address to firewall, InterfaceAlias is your Network Adapter's name
$ipRangeFilter = Get-NetIPConfiguration | Where-Object InterfaceAlias -eq "Ethernet 2" | Select-Object IPv4Address

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "ipRangeFilter"=$ipRangeFilter
}

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a id="troubleshoot-ip-firewall"></a>IP erişim denetimi ilkesiyle ilgili sorunları giderme

Aşağıdaki seçenekleri kullanarak bir IP erişim denetimi ilkesiyle ilgili sorunları çözebilirsiniz: 

### <a name="azure-portal"></a>Azure portal 
Azure Cosmos DB hesabınız için bir IP erişim denetimi ilkesi etkinleştirerek, hesabınıza verilen IP adresi aralıkları listesinin dışındaki tüm istekleri engellenir. Kapsayıcılara göz atma ve belge sorgulama gibi portal veri düzlemi işlemlerini etkinleştirmek için, portalda **güvenlik duvarı** bölmesini kullanarak Azure Portal erişime açıkça izin vermeniz gerekir.

### <a name="sdks"></a>SDK’lar 
İzin verilenler listesinde olmayan makinelerden SDK 'Ları kullanarak Azure Cosmos DB kaynaklarına eriştiğinizde, ek ayrıntı olmadan genel **403 yasaklanmış** bir yanıt döndürülür. Hesabınız için izin verilen IP listesini doğrulayın ve Azure Cosmos DB hesabınıza doğru ilke yapılandırmasının uygulandığından emin olun. 

### <a name="source-ips-in-blocked-requests"></a>Engellenen isteklerde kaynak IP 'Leri
Azure Cosmos DB hesabınızda tanılama günlüğünü etkinleştirin. Bu Günlükler her isteği ve yanıtı gösterir. Güvenlik Duvarı ile ilgili iletiler 403 dönüş koduyla günlüğe kaydedilir. Bu iletileri filtreleyerek, engellenen isteklerin kaynak IP 'lerini görebilirsiniz. Bkz. [Azure Cosmos DB tanılama günlüğü](logging.md).

### <a name="requests-from-a-subnet-with-a-service-endpoint-for-azure-cosmos-db-enabled"></a>Azure Cosmos DB etkin için bir hizmet uç noktası olan bir alt ağdan gelen istekler
Azure Cosmos DB etkin için bir hizmet uç noktası olan bir sanal ağ alt ağından gelen istekler, sanal ağ ve alt ağ kimliğini Azure Cosmos DB hesaplara gönderir. Bu istekler, kaynağın genel IP 'si içermez, bu nedenle IP filtreleri bunları reddeder. Sanal ağlardaki belirli alt ağlardan erişime izin vermek için, [Azure Cosmos DB hesabınız için sanal ağ ve alt ağ tabanlı erişimin nasıl yapılandırılacağı konusunda](how-to-configure-vnet-service-endpoint.md)özetlenen bir erişim denetim listesi ekleyin. Güvenlik Duvarı kurallarının uygulanması 15 dakika kadar sürebilir.


## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB hesabınız için bir sanal ağ hizmeti uç noktası yapılandırmak için aşağıdaki makalelere bakın:

* [Azure Cosmos DB hesabınız için sanal ağ ve alt ağ erişim denetimi](vnet-service-endpoint.md)
* [Azure Cosmos DB hesabınız için sanal ağ ve alt ağ tabanlı erişimi yapılandırın](how-to-configure-vnet-service-endpoint.md)

