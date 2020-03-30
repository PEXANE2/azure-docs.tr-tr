---
title: Güvenlik duvarı erişim kuralları
description: REST API ve depolama bitiş noktası etki alanı adlarına veya hizmete özgü IP adresi aralıklarına erişime izin vererek, bir Güvenlik Duvarı'nın arkasından Bir Azure kapsayıcı kayıt defterine erişmek için kuralları yapılandırın.
ms.topic: article
ms.date: 02/11/2020
ms.openlocfilehash: 06fedea2adf5e73929f5752279f2bd7e7227e570
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77168014"
---
# <a name="configure-rules-to-access-an-azure-container-registry-behind-a-firewall"></a>Güvenlik duvarının arkasındaki Azure kapsayıcı kayıt defterine erişmek için kuralları yapılandırma

Bu makalede, bir Azure kapsayıcı kayıt defterine erişime izin vermek için güvenlik duvarınızdaki kuralları nasıl yapılandırılacak açıklanmaktadır. Örneğin, bir güvenlik duvarının veya proxy sunucusunun arkasındaki bir Azure IoT Edge aygıtının kapsayıcı görüntüsünü çekmek için bir kapsayıcı kayıt defterine erişmesi gerekebilir. Veya, şirket içi bir ağdaki kilitli bir sunucunun görüntüyü itmek için erişimi gerekebilir.

Bunun yerine, yalnızca Bir Azure sanal ağında veya genel bir IP adresi aralığından gelen bir kapsayıcı kayıt defterinde gelen ağ erişim kurallarını yapılandırmak istiyorsanız, [bkz.](container-registry-vnet.md)

## <a name="about-registry-endpoints"></a>Kayıt defteri bitiş noktaları hakkında

Görüntüleri veya diğer yapıları Azure kapsayıcı kayıt defterine çekmek veya itmek için Docker daemon gibi bir istemcinin IKI farklı uç noktayla HTTPS üzerinden etkileşimde olması gerekir.

* **Kayıt DEFTERI REST API bitiş noktası** - Kimlik doğrulama ve kayıt defteri yönetimi işlemleri, kayıt defterinin genel REST API bitiş noktası üzerinden gerçekleştirilir. Bu bitiş noktası, kayıt defterinin giriş sunucusu adı veya ilişkili bir IP adresi aralığıdır. 

* **Depolama bitiş noktası** - Azure, kapsayıcı resimleri ve diğer yapılara ait verileri yönetmek için her kayıt defteri adına Azure Depolama [hesaplarındaki blob depolama](container-registry-storage.md) alanını ayırır. İstemci, Azure kapsayıcı kayıt defterinde görüntü katmanlarına erişiyorsa, kayıt defteri tarafından sağlanan bir depolama hesabı bitiş noktasını kullanarak isteklerde bulundur.

Kayıt defteriniz [coğrafi olarak çoğaltılmışsa,](container-registry-geo-replication.md)istemcinin belirli bir bölgedeki VEYA birden çok çoğaltılan bölgede REST ve depolama bitiş noktalarıyla etkileşimde olması gerekebilir.

## <a name="allow-access-to-rest-and-storage-domain-names"></a>REST ve depolama alan adlarına erişime izin ver

* **REST endpoint** - Tam nitelikli kayıt defteri giriş sunucusu adına erişime izin ver, örneğin`myregistry.azurecr.io`
* **Depolama (veri) bitiş noktası** - Joker kartı kullanarak tüm Azure blob depolama hesaplarına erişme`*.blob.core.windows.net`


## <a name="allow-access-by-ip-address-range"></a>IP adresi aralığına göre erişime izin ver

Kuruluşunuzun yalnızca belirli IP adreslerine veya adres aralıklarına erişime izin veren politikaları varsa, [Azure IP Aralıkları ve Hizmet Etiketleri indir](https://www.microsoft.com/download/details.aspx?id=56519)– Genel Bulut .

Erişime izin vermeniz gereken ACR REST uç nokta IP aralıklarını bulmak için JSON dosyasında **AzureContainerRegistry'yi** arayın.

> [!IMPORTANT]
> Azure hizmetleri için IP adres aralıkları değişebilir ve güncelleştirmeler haftalık olarak yayınlanır. JSON dosyasını düzenli olarak indirin ve erişim kurallarınızda gerekli güncelleştirmeleri yapın. Senaryonuz, Azure Konteyner Kayıt Defteri'ne erişmek için bir Azure sanal ağındaki ağ güvenliği grubu kurallarını yapılandırmayı içeriyorsa, bunun yerine **Azure Container Registry** [hizmet etiketini](#allow-access-by-service-tag) kullanın.
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

**AzureContainerRegistry.AustraliaEast**gibi belirli bir bölgeyi arayın.

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

**Storage.AustraliaCentral**gibi belirli bir bölgeyi arayın.

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

Azure sanal ağında, sanal makine gibi bir kaynaktan konteyner kayıt defterine kadar olan trafiği filtrelemek için ağ güvenliği kurallarını kullanın. Azure ağ kurallarının oluşturulmasını kolaylaştırmak için **AzureContainerRegistry** [hizmet etiketini](../virtual-network/security-overview.md#service-tags)kullanın. Hizmet etiketi, bir Azure hizmetine genel olarak veya Azure bölgesi başına erişmek için bir ip adresi önekleri grubunu temsil eder. Adresler değiştiğinde etiket otomatik olarak güncelleştirilir. 

Örneğin, bir Azure kapsayıcı kayıt defterine trafik izni vermek için hedef **AzureContainerRegistry** ile giden ağ güvenlik grubu kuralı oluşturun. Yalnızca belirli bir bölgede hizmet etiketine erişime izin vermek için, bölgeyi aşağıdaki biçimde belirtin: **AzureContainerRegistry**. [*bölge adı*].

## <a name="configure-client-firewall-rules-for-mcr"></a>MCR için istemci güvenlik duvarı kurallarını yapılandırma

Bir güvenlik duvarının arkasından Microsoft Konteyner Kayıt Defteri'ne (MCR) erişmeniz gerekiyorsa, [MCR istemci güvenlik duvarı kurallarını](https://github.com/microsoft/containerregistry/blob/master/client-firewall-rules.md)yapılandırma kılavuzuna bakın. MCR, Windows Server görüntüleri gibi Microsoft tarafından yayımlanan tüm docker görüntülerinin birincil kayıt defteridir.

## <a name="next-steps"></a>Sonraki adımlar

* Ağ [güvenliği için Azure en iyi uygulamaları](../security/fundamentals/network-best-practices.md) hakkında bilgi edinin

* Azure sanal ağındaki [güvenlik grupları](/azure/virtual-network/security-overview) hakkında daha fazla bilgi edinin



<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->

