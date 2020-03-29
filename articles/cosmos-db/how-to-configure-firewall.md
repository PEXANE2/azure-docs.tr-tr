---
title: Azure Cosmos DB hesabınız için bir IP güvenlik duvarı yapılandırma
description: Azure Cosmos hesaplarında güvenlik duvarı desteği için IP erişim denetimi ilkelerini nasıl yapılandırılamayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: mjbrown
ms.openlocfilehash: 1c24782285ac9b06d5499351eebe1693ade07297
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78162953"
---
# <a name="configure-ip-firewall-in-azure-cosmos-db"></a>Azure Cosmos DB'de IP güvenlik duvarını yapılandırma

IP güvenlik duvarlarını kullanarak Azure Cosmos DB hesabınızda depolanan verileri güvenli hale alabilirsiniz. Azure Cosmos DB, gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Aşağıdaki yollardan birini kullanarak Azure Cosmos DB hesabında bir IP güvenlik duvarı ayarlayabilirsiniz:

* Azure portalından
* Azure Resource Manager şablonu kullanarak bildirimli olarak
* **ipRangeFilter** özelliğini güncelleyerek Azure CLI veya Azure PowerShell üzerinden programlanabilir şekilde

## <a name="configure-an-ip-firewall-by-using-the-azure-portal"></a><a id="configure-ip-policy"></a>Azure portalını kullanarak bir IP güvenlik duvarı yapılandırma

Azure portalındaki IP erişim denetim ilkesini ayarlamak için Azure Cosmos DB hesap sayfasına gidin ve gezinti menüsünde **Güvenlik Duvarı ve sanal ağları** seçin. **Erişime değerden** **Seçili ağlara**geçiş izni ver ve ardından **Kaydet'i**seçin.

![Azure portalında Güvenlik Duvarı sayfasının nasıl açılacağını gösteren ekran görüntüsü](./media/how-to-configure-firewall/azure-portal-firewall.png)

IP erişim denetimi açık olduğunda, Azure portalı IP adreslerini, IP adres aralıklarını ve anahtarlarını belirtme olanağı sağlar. Anahtarlar, diğer Azure hizmetlerine ve Azure portalına erişimi sağlar. Aşağıdaki bölümlerde bu anahtarlar hakkında ayrıntılı bilgi verilmiştir.

> [!NOTE]
> Azure Cosmos DB hesabınız için bir IP erişim kontrol ilkesini etkinleştirdikten sonra, izin verilen IP adresi aralıkları listesidışındaki makinelerden Azure Cosmos DB hesabınıza gelen tüm istekler reddedilir. Portaldan Azure Cosmos DB kaynaklarına göz atmak da erişim denetiminin bütünlüğünü sağlamak için engellenir.

### <a name="allow-requests-from-the-azure-portal"></a>Azure portalından isteklere izin verme

Bir IP erişim denetim ilkesini programlı bir şekilde etkinleştirdiğinizde, erişimi korumak için Azure portalının IP adresini **ipRangeFilter** özelliğine eklemeniz gerekir. Portal IP adresleri şunlardır:

|Bölge|IP adresi|
|------|----------|
|Almanya|51.4.229.218|
|Çin|139.217.8.252|
|US Gov|52.244.48.71|
|Diğer tüm bölgeler|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure **portalından erişime izin ver** seçeneğini seçerek Azure portalına erişim isteği etkinleştirebilirsiniz:

![Azure portal erişimini nasıl etkinleştireceklerini gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Azure içinde küresel Azure veri merkezlerinden veya başka kaynaklardan gelen isteklere izin verme

Azure Cosmos DB hesabınıza statik IP sağlamayan hizmetlerden (örneğin, Azure Akış Analizi ve Azure İşlevleri) eriştiyseniz, erişimi sınırlamak için IP güvenlik duvarını kullanmaya devam edebilirsiniz. Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure **veri merkezleri içinden bağlantıları Kabul** et seçeneğini seçerek Azure'daki diğer kaynaklardan erişimi etkinleştirebilirsiniz:

![Azure portalında Güvenlik Duvarı sayfasının nasıl açılacağını gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-azure-services.png)

Bu seçeneği etkinleştirdiğinizde, IP `0.0.0.0` adresi izin verilen IP adresleri listesine eklenir. IP `0.0.0.0` adresi, Azure veri merkezi IP aralığından Azure Cosmos DB hesabınızdaki istekleri kısıtlar. Bu ayar Azure Cosmos DB hesabınıza başka hiçbir IP aralığından erişime izin vermez.

> [!NOTE]
> Bu seçenek, güvenlik duvarını Azure'da dağıtılan diğer müşterilerin abonelikleri de dahil olmak üzere Azure'dan gelen tüm isteklere izin verecek şekilde yapılandırır. Bu seçeneğin izin verdiği IP'lerin listesi geniştir, bu nedenle güvenlik duvarı ilkesinin etkinliğini sınırlar. Bu seçeneği yalnızca istekleriniz sanal ağlardaki statik IP'lerden veya alt ağlardan kaynaklanmıyorsa kullanın. Azure portalı Azure'da dağıtıldığı için bu seçeneğin seçilmesi Azure portalından otomatik olarak erişime izin verir.

### <a name="requests-from-your-current-ip"></a>Geçerli IP'nizden gelen istekler

Azure portalı, geliştirmeyi kolaylaştırmak için istemci makinenizin IP'sini belirlemenize ve izin verilen listeye eklemenize yardımcı olur. Makinenizi çalıştıran uygulamalar daha sonra Azure Cosmos DB hesabınıza erişebilir.

Portal istemci IP adresini otomatik olarak algılar. Makinenizin istemci IP adresi veya ağ ağ geçidinizin IP adresi olabilir. İş yüklerinizi üretime götürmeden önce bu IP adresini kaldırdığınızdan emin olun.

Geçerli IP'nizi IP listesine eklemek için **geçerli IP'mi ekle'yi**seçin. Ardından **Kaydet'i**seçin.

![Geçerli IP için güvenlik duvarı ayarlarını nasıl yapılandırılabildiğini gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Bulut hizmetlerinden gelen talepler

Azure'da bulut hizmetleri, Azure Cosmos DB'yi kullanarak orta katman hizmet mantığına ev sahipliği yapmanın yaygın bir yoludur. Azure Cosmos DB hesabınıza bir bulut hizmetinden erişimi etkinleştirmek için, [IP erişim kontrol ilkesini yapılandırarak](#configure-ip-policy)Bulut hizmetinin genel IP adresini Azure Cosmos DB hesabınızla ilişkili IP adresleri listesine eklemeniz gerekir. Bu, bulut hizmetlerinin tüm rol örneklerinin Azure Cosmos DB hesabınıza erişmesini sağlar.

Bulut hizmetlerinizin IP adreslerini Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalından alabilirsiniz:

![Azure portalında görüntülenen bir bulut hizmetinin genel IP adresini gösteren ekran görüntüsü](./media/how-to-configure-firewall/public-ip-addresses.png)

Rol örnekleri ekleyerek bulut hizmetinizi ölçeklendirdiğinizde, bu yeni örnekler aynı bulut hizmetinin bir parçası oldukları için Azure Cosmos DB hesabına otomatik olarak erişebilir.

### <a name="requests-from-virtual-machines"></a>Sanal makinelerden gelen istekler

Azure Cosmos DB'yi kullanarak orta katman hizmetlerine ev sahipliği yapmak için [sanal makineleri](https://azure.microsoft.com/services/virtual-machines/) veya sanal makine [ölçeği kümelerini](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) de kullanabilirsiniz. Cosmos DB hesabınızı sanal makinelerden erişime izin sağlayacak şekilde yapılandırmak için, SANAL MAKINE'nin genel IP adresini ve/veya sanal makine [ölçeğini, IP erişim kontrol ilkesini yapılandırarak](#configure-ip-policy)Azure Cosmos DB hesabınız için izin verilen IP adreslerinden biri olarak yapılandırmanız gerekir.

Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında sanal makinelerin IP adreslerini alabilirsiniz:

![Azure portalında görüntülenen sanal bir makinenin genel IP adresini gösteren ekran görüntüsü](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Gruba sanal makine örnekleri eklediğinizde, Azure Cosmos DB hesabınıza otomatik olarak erişirler.

### <a name="requests-from-the-internet"></a>İnternetten gelen talepler

Azure Cosmos DB hesabınıza internet üzerindeki bir bilgisayardan eriştikte, makinenin istemci IP adresi veya IP adresi aralığı, hesabınız için izin verilen IP adresleri listesine eklenmelidir.

## <a name="configure-an-ip-firewall-by-using-a-resource-manager-template"></a><a id="configure-ip-firewall-arm"></a>Kaynak Yöneticisi şablonu kullanarak IP güvenlik duvarını yapılandırma

Azure Cosmos DB hesabınıza erişim denetimini yapılandırmak için, Kaynak Yöneticisi şablonunun **ipRangeFilter** özniteliğini izin verilen IP aralıklarının bir listesiyle belirttiğinden emin olun. IP Güvenlik Duvarı'nı zaten dağıtılmış bir Cosmos `locations` hesabına yapılandırıyorsanız, dizinin şu anda dağıtılan hesapla eşleştiğinden emin olun. Diziyi ve diğer `locations` özellikleri aynı anda değiştiremezsiniz. Azure Cosmos DB için daha fazla bilgi ve Azure Kaynak Yöneticisi şablonları örnekleri için [bkz.](resource-manager-samples.md)

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

## <a name="configure-an-ip-access-control-policy-by-using-the-azure-cli"></a><a id="configure-ip-firewall-cli"></a>Azure CLI'yi kullanarak BIR IP erişim denetim ilkesini yapılandırma

Aşağıdaki komut, IP erişim denetimine sahip bir Azure Cosmos DB hesabının nasıl oluşturulur unu gösterir:

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

## <a name="configure-an-ip-access-control-policy-by-using-powershell"></a><a id="configure-ip-firewall-ps"></a>PowerShell'i kullanarak IP erişim denetim ilkesini yapılandırma

Aşağıdaki komut dosyası, IP erişim denetimine sahip bir Azure Cosmos DB hesabının nasıl oluşturulabildiğini gösterir:

```azurepowershell-interactive
# Create a Cosmos DB account with default values and IP Firewall enabled
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$ipRangeFilter = "192.168.221.17,183.240.196.255,40.76.54.131"

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0; "isZoneRedundant"=False },
    @{ "locationName"="East US 2"; "failoverPriority"=1, "isZoneRedundant"=False }
)

# Make sure there are no spaces in the comma-delimited list of IP addresses or CIDR ranges.
$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "ipRangeFilter"=$ipRangeFilter
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="troubleshoot-issues-with-an-ip-access-control-policy"></a><a id="troubleshoot-ip-firewall"></a>IP erişim denetim ilkesiyle sorunları giderme

Aşağıdaki seçenekleri kullanarak BIR IP erişim denetim ilkesi yle ilgili sorunları giderebilirsiniz:

### <a name="azure-portal"></a>Azure portalında

Azure Cosmos DB hesabınız için bir IP erişim denetim ilkesi etkinleştirerek, izin verilen IP adres aralıkları listesidışındaki makinelerden hesabınıza yönelik tüm istekleri engellersiniz. Kapsayıcılara göz atma ve belgeleri sorgulama gibi portal veri düzlemi işlemlerini etkinleştirmek için, portaldaki **Güvenlik Duvarı** bölmesini kullanarak Azure portalıerişimine açıkça izin vermeniz gerekir.

### <a name="sdks"></a>SDK’lar

Azure Cosmos DB kaynaklarına izin verilenler listesinde olmayan makinelerden SDK'lar kullanarak eriştiyseniz, genel bir **403 Forbidden** yanıtı ek ayrıntı olmadan döndürülür. Hesabınız için izin verilen IP listesini doğrulayın ve Azure Cosmos DB hesabınıza doğru ilke yapılandırmasının uygulandığından emin olun.

### <a name="source-ips-in-blocked-requests"></a>Engellenen isteklerde Kaynak IP'ler

Azure Cosmos DB hesabınızda tanısal günlüğe kaydetmeyi etkinleştirin. Bu günlükler her isteği ve yanıtı gösterir. Güvenlik duvarıyla ilgili iletiler 403 iade koduyla günlüğe kaydedilir. Bu iletileri filtreleyerek, engellenen isteklerin kaynak IP'lerini görebilirsiniz. Bkz. [Azure Cosmos DB tanılama günlüğü.](logging.md)

### <a name="requests-from-a-subnet-with-a-service-endpoint-for-azure-cosmos-db-enabled"></a>Azure Cosmos DB için hizmet bitiş noktası etkin leştirilmiş bir alt ağdan gelen istekler

Azure Cosmos DB için hizmet bitiş noktası etkin olan sanal ağdaki bir alt ağdan gelen istekler, sanal ağı ve alt ağ kimliğini Azure Cosmos DB hesaplarına gönderir. Bu istekler kaynağın genel IP'si yoktur, bu nedenle IP filtreleri bunları reddeder. Sanal ağlardaki belirli alt ağlardan erişime izin vermek [için, Azure Cosmos DB hesabınız için sanal ağı ve alt ağ tabanlı erişimi yapılandırma da](how-to-configure-vnet-service-endpoint.md)özetlenen bir erişim denetim listesi ekleyin. Güvenlik duvarı kurallarının uygulanması 15 dakika kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB hesabınız için sanal ağ hizmeti bitiş noktasını yapılandırmak için aşağıdaki makalelere bakın:

* [Azure Cosmos DB hesabınız için sanal ağ ve alt ağ erişim denetimi](vnet-service-endpoint.md)
* [Azure Cosmos DB hesabınız için sanal ağı ve alt ağ tabanlı erişimi yapılandırın](how-to-configure-vnet-service-endpoint.md)
