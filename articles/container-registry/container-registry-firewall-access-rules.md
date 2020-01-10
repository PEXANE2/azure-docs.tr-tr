---
title: Güvenlik Duvarı erişim kuralları
description: ("Beyaz listeye") REST API ve depolama uç noktası etki alanı adlarına veya hizmete özel IP adresi aralıklarına erişime izin vererek bir güvenlik duvarının arkasındaki bir Azure Container Registry 'ye erişmek için kuralları yapılandırın.
ms.topic: article
ms.date: 07/17/2019
ms.openlocfilehash: 4d3c4ff4ca19d8b563c185e5c314011823081df1
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75745193"
---
# <a name="configure-rules-to-access-an-azure-container-registry-behind-a-firewall"></a>Güvenlik duvarının arkasındaki bir Azure Container Registry 'ye erişmek için kuralları yapılandırma

Bu makalede, Azure Container Registry 'ye erişime izin vermek için güvenlik duvarınızdaki kuralların nasıl yapılandırılacağı açıklanır. Örneğin, bir güvenlik duvarı veya proxy sunucusu arkasındaki bir Azure IoT Edge cihazın bir kapsayıcı görüntüsünü çekmek için bir kapsayıcı kayıt defterine erişmesi gerekebilir. Ya da, bir şirket içi ağdaki kilitlenmiş bir sunucunun bir görüntüyü göndermek için erişmesi gerekebilir.

Bunun yerine, bir kapsayıcı kayıt defterinde yalnızca bir Azure sanal ağı veya genel IP adres aralığından gelen ağ erişim kurallarını yapılandırmak istiyorsanız bkz. [bir Azure Container Registry 'ye erişimi bir sanal ağdan kısıtlama](container-registry-vnet.md).

## <a name="about-registry-endpoints"></a>Kayıt defteri uç noktaları hakkında

Bir Azure Container Registry 'ye görüntü veya diğer yapıtları çekmek veya göndermek için, Docker Daemon gibi bir istemcinin iki ayrı uç nokta ile HTTPS üzerinden etkileşimde bulunması gerekir.

* **Kayıt defteri REST API uç noktası** -kimlik doğrulama ve kayıt defteri yönetim işlemleri kayıt defterinin ortak REST API uç noktası aracılığıyla işlenir. Bu uç nokta, kayıt defterinin oturum açma sunucusu adı veya ilişkili bir IP adresi aralığıdır. 

* **Depolama uç noktası** -Azure, kapsayıcı görüntüleri ve diğer yapıtlar için verileri yönetmek üzere her kayıt defteri adına Azure Storage hesaplarında [BLOB depolamayı ayırır](container-registry-storage.md) . Bir istemci bir Azure Container Registry içindeki görüntü katmanlarına eriştiğinde, kayıt defteri tarafından sağlanmış bir depolama hesabı uç noktası kullanarak istek yapar.

Kayıt defteriniz coğrafi olarak [çoğaltılırsa](container-registry-geo-replication.md), bir istemcinin belirli bir bölgedeki veya birden çok ÇOĞALTıLAN bölgedeki REST ve depolama uç noktalarıyla etkileşimde olması gerekebilir.

## <a name="allow-access-to-rest-and-storage-domain-names"></a>REST ve depolama alanı adlarına erişime izin ver

* **REST uç noktası** -`myregistry.azurecr.io` gibi tam kayıt defteri oturum açma sunucu adına erişime izin ver
* **Depolama (veri) uç noktası** -joker karakter `*.blob.core.windows.net` kullanarak tüm Azure Blob depolama hesaplarına erişime izin ver


## <a name="allow-access-by-ip-address-range"></a>IP adresi aralığına göre erişime izin ver

Kuruluşunuzun yalnızca belirli IP adreslerine veya adres aralıklarına erişime izin vermek için ilkeler varsa, [Azure IP aralıklarını ve hizmet etiketleri – genel bulut](https://www.microsoft.com/download/details.aspx?id=56519)' u indirin.

Erişime izin vermeniz gereken ACR REST uç nokta IP aralıklarını bulmak için JSON dosyasında **AzureContainerRegistry** aratın.

> [!IMPORTANT]
> Azure hizmetleri için IP adresi aralıkları değişebilir ve güncelleştirmeler haftalık olarak yayımlanır. JSON dosyasını düzenli olarak indirin ve erişim kurallarınızın gerekli güncelleştirmelerini yapın. Senaryonuz Azure Container Registry erişmek için bir Azure sanal ağındaki ağ güvenlik grubu kurallarını yapılandırmayı içeriyorsa, bunun yerine **AzureContainerRegistry** [Service etiketini](#allow-access-by-service-tag) kullanın.
>

### <a name="rest-ip-addresses-for-all-regions"></a>Tüm bölgeler için REST IP adresleri

```json
{
  "name": "AzureContainerRegistry",
  "id": "AzureContainerRegistry",
  "properties": {
    "changeNumber": 10,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "13.66.140.72/29",
    [...]
```

### <a name="rest-ip-addresses-for-a-specific-region"></a>Belirli bir bölge için REST IP adresleri

**AzureContainerRegistry. AustraliaEast**gibi belirli bir bölgeyi arayın.

```json
{
  "name": "AzureContainerRegistry.AustraliaEast",
  "id": "AzureContainerRegistry.AustraliaEast",
  "properties": {
    "changeNumber": 1,
    "region": "australiaeast",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "13.70.72.136/29",
    [...]
```

### <a name="storage-ip-addresses-for-all-regions"></a>Tüm bölgeler için depolama IP adresleri

```json
{
  "name": "Storage",
  "id": "Storage",
  "properties": {
    "changeNumber": 19,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "13.65.107.32/28",
    [...]
```

### <a name="storage-ip-addresses-for-specific-regions"></a>Belirli bölgeler için depolama IP adresleri

**Storage. AustraliaCentral**gibi belirli bir bölgeyi arayın.

```json
{
  "name": "Storage.AustraliaCentral",
  "id": "Storage.AustraliaCentral",
  "properties": {
    "changeNumber": 1,
    "region": "australiacentral",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "52.239.216.0/23"
    [...]
```

## <a name="allow-access-by-service-tag"></a>Hizmet etiketine göre erişime izin ver

Bir Azure sanal ağında, sanal makine gibi bir kaynaktan bir kapsayıcı kayıt defterine giden trafiği filtrelemek için ağ güvenlik kurallarını kullanın. Azure ağ kurallarının oluşturulmasını basitleştirmek için **AzureContainerRegistry** [Service etiketini](../virtual-network/security-overview.md#service-tags)kullanın. Hizmet etiketi, bir Azure hizmetine küresel olarak veya Azure bölgesine erişmek için bir IP adresi ön eki grubunu temsil eder. Adresler değiştiğinde etiket otomatik olarak güncelleştirilir. 

Örneğin, Azure Container Registry 'ye giden trafiğe izin vermek için hedef **AzureContainerRegistry** ile giden ağ güvenlik grubu kuralı oluşturun. Yalnızca belirli bir bölgedeki hizmet etiketine erişime izin vermek için, bölgeyi şu biçimde belirtin: **AzureContainerRegistry**. [*bölge adı*].

## <a name="next-steps"></a>Sonraki adımlar

* [Ağ güvenliği için En Iyi Azure uygulamaları](../security/fundamentals/network-best-practices.md) hakkında bilgi edinin

* Azure sanal ağındaki [güvenlik grupları](/azure/virtual-network/security-overview) hakkında daha fazla bilgi edinin



<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->

