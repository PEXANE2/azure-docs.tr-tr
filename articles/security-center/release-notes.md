---
title: Azure Güvenlik Merkezi için sürüm notları
description: Azure Güvenlik Merkezi 'nde nelerin yeni ve değiştirilmiş olduğuna ilişkin bir açıklama.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2020
ms.author: memildin
ms.openlocfilehash: 375dff1dacc949dd4373bbf26908feb504750224
ms.sourcegitcommit: 65d518d1ccdbb7b7e1b1de1c387c382edf037850
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94372360"
---
# <a name="whats-new-in-azure-security-center"></a>Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?

Güvenlik Merkezi, etkin geliştirme aşamasındadır ve gelişmede geliştirmeler sunar. En son gelişmelerden haberdar olmak için bu sayfada yeni özellikler, hata düzeltmeleri ve kullanım dışı işlevler hakkında bilgi verilmektedir.

Bu sayfa sıklıkla güncelleştirildi, bu nedenle sık olarak yeniden ziyaret edin. 

Güvenlik Merkezi 'ne yakında çıkacak *planlı* değişiklikler hakkında bilgi edinmek için bkz. [Azure Güvenlik Merkezi 'nde yapılan önemli değişiklikler](upcoming-changes.md). 

> [!TIP]
> Altı aydan eski olan öğeleri arıyorsanız, [Azure Güvenlik Merkezi 'ndeki yenilikler Için arşivde](release-notes-archive.md)bulabilirsiniz.


## <a name="november-2020"></a>Kasım 2020

Kasım 'daki güncelleştirmeler şunlardır:

- [Azure Güvenlik kıyaslamasının kapsamını artırmak için 29 önizleme önerileri eklenmiştir](#29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark)
- [NıST SP 800 171 R2, güvenlik merkezi 'nin mevzuat uyumluluk panosuna eklendi](#nist-sp-800-171-r2-added-to-security-centers-regulatory-compliance-dashboard)


### <a name="29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark"></a>Azure Güvenlik kıyaslamasının kapsamını artırmak için 29 önizleme önerileri eklenmiştir

Azure Güvenlik kıyaslaması, yaygın uyumluluk çerçevelerine göre güvenlik ve uyum en iyi uygulamaları için Microsoft tarafından yazılan, Azure 'a özgü bir dizi kılavuzlardır. [Azure Güvenlik Karşılaştırması hakkında daha fazla bilgi edinin](../security/benchmarks/introduction.md).

Bu kıyaslama kapsamını artırmak için Güvenlik Merkezi 'ne aşağıdaki 29 önizleme önerileri eklenmiştir.

Önizleme önerileri bir kaynağı sağlıksız bir şekilde işlemez ve güvenli puanınızın hesaplamalarına dahil değildir. Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunmak için bunları mümkün olduğunda düzeltin. [Azure Güvenlik Merkezi 'ndeki önerileri düzeltin](security-center-remediate-recommendations.md)bölümünde bu önerilere yanıt verme hakkında daha fazla bilgi edinin.

| Güvenlik denetimi                     | Yeni öneriler                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yoldaki verileri şifreleme              | -PostgreSQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir<br>-MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir<br>-TLS API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-TLS, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-TLS, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-FTPS API uygulamanızda gerekli olmalıdır<br>-FTPS, işlev uygulamanızda gerekli olmalıdır<br>-FTPS Web uygulamanızda gerekli olmalıdır                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Erişimi ve izinleri yönetme        | -Web uygulamaları tüm gelen istekler için bir SSL sertifikası istemelidir<br>-Yönetilen kimlik API uygulamanızda kullanılmalıdır<br>-Yönetilen kimlik, işlev uygulamanızda kullanılmalıdır<br>-Yönetilen kimliğin Web uygulamanızda kullanılması gerekir                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Yetkisiz ağ erişimini kısıtla | -PostgreSQL sunucuları için özel uç noktanın etkinleştirilmesi gerekir<br>-Özel uç noktanın MariaDB sunucuları için etkinleştirilmesi gerekir<br>-MySQL sunucuları için özel uç noktanın etkinleştirilmesi gerekir                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Denetim ve günlüğe kaydetmeyi etkinleştirme          | -Uygulama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| En iyi güvenlik uygulamalarını uygulayın    | -Azure Backup sanal makineler için etkinleştirilmelidir<br>-Coğrafi olarak yedekli yedekleme, MariaDB için Azure veritabanı 'nda etkinleştirilmelidir<br>-MySQL için Azure veritabanı için coğrafi olarak yedekli yedeklemenin etkinleştirilmesi gerekir<br>-PostgreSQL için Azure veritabanı için coğrafi olarak yedekli yedeklemenin etkinleştirilmesi gerekir<br>-PHP, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-PHP, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Java, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Java, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Java, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Python, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Python, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Python, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-SQL Server 'lar için denetim bekletme en az 90 gün olarak ayarlanmalıdır |
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

İlgili bağlantılar:

- [Azure Güvenlik kıyaslaması hakkında daha fazla bilgi](../security/benchmarks/introduction.md)
- [Azure API Apps hakkında daha fazla bilgi](../app-service/app-service-web-tutorial-rest-api.md)
- [Azure işlev uygulamaları hakkında daha fazla bilgi edinin](../azure-functions/functions-overview.md)
- [Azure Web Apps hakkında daha fazla bilgi](../app-service/overview.md)
- [MariaDB için Azure veritabanı hakkında daha fazla bilgi edinin](../mariadb/overview.md)
- [MySQL için Azure veritabanı hakkında daha fazla bilgi edinin](../mysql/overview.md)
- [PostgreSQL için Azure veritabanı hakkında daha fazla bilgi edinin](../postgresql/overview.md)


### <a name="nist-sp-800-171-r2-added-to-security-centers-regulatory-compliance-dashboard"></a>NıST SP 800 171 R2, güvenlik merkezi 'nin mevzuat uyumluluk panosuna eklendi

NıST SP 800-171 R2 Standard artık, Azure Güvenlik Merkezi 'nin mevzuat uyumluluk panosu ile birlikte kullanılmak üzere yerleşik bir girişim olarak sunulmaktadır. Denetimlerle ilgili eşlemeler, [NıST SP 800-171 R2 mevzuatı uyumluluğu yerleşik girişiminin ayrıntıları](../governance/policy/samples/nist-sp-800-171-r2.md)bölümünde açıklanmaktadır. 

Standart aboneliğinizi aboneliklerinize uygulamak ve uyumluluk durumunuzu sürekli olarak izlemek için, [mevzuat uyumluluk panonuzdaki standartlar kümesini özelleştirme](update-regulatory-compliance-packages.md)bölümündeki yönergeleri kullanın.

:::image type="content" source="media/release-notes/nist-sp-800-171-r2-standard.png" alt-text="Güvenlik Merkezi 'nin mevzuat uyumluluk panosundaki NıST SP 800 171 R2 Standard":::

Bu uyumluluk standardı hakkında daha fazla bilgi için bkz. [NıST SP 800-171 R2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final).



## <a name="october-2020"></a>Ekim 2020

Ekim 'deki güncelleştirmeler şunlardır:
- [Şirket içi ve çoklu bulut makinelerinde güvenlik açığı değerlendirmesi (Önizleme)](#vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview)
- [Azure Güvenlik Duvarı önerisi eklendi (Önizleme)](#azure-firewall-recommendation-added-preview)
- [Yetkili IP aralıklarının, hızlı düzeltmeyle güncelleştirilmiş Kubernetes Hizmetleri önerisi üzerinde tanımlanması gerekir](#authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix)
- [Mevzuat uyumluluk panosu artık standartları kaldırma seçeneğini içermektedir](#regulatory-compliance-dashboard-now-includes-option-to-remove-standards)
- [Microsoft. Security/Securitydurumlarının tablosu Azure Kaynak grafiğinden kaldırıldı (ARG)](#microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg)

### <a name="vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview"></a>Şirket içi ve çoklu bulut makinelerinde güvenlik açığı değerlendirmesi (Önizleme)

[Sunucular Için Azure Defender 'ın](defender-for-servers-introduction.md)tümleşik güvenlik açığı değerlendirmesi tarayıcısı (Qualys tarafından desteklenir) artık Azure Arc etkin sunucularını tarar.

Azure dışı makinelerinizde Azure yayı 'yi etkinleştirdiyseniz, Güvenlik Merkezi, tümleşik güvenlik açığı tarayıcısını bunlara el ile ve ölçekli olarak dağıtmayı sağlar.

Bu güncelleştirmeyle, Azure Defender 'ın tüm Azure ve Azure dışı varlıklarınızda güvenlik açığı yönetimi programınızı birleştirmek için **Azure Defender** 'ın gücünü açığa çıkarın.

Ana yetenekler:

- Azure Arc makinelerinde VA (güvenlik açığı değerlendirmesi) tarayıcı sağlama durumunu izleme
- Tümleşik VA aracısını korumasız Windows ve Linux Azure yay makinelerine sağlama (el ile ve ölçekli)
- Dağıtılan aracılardan algılanan güvenlik açıklarını alma ve çözümleme (el ile ve ölçekli)
- Azure sanal makineleri ve Azure yay makineleri için Birleşik deneyim

[Tümleşik güvenlik açığı tarayıcısını karma makinelerinize dağıtma hakkında daha fazla bilgi edinin](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines).

[Azure Arc etkin sunucuları hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-arc/servers/).


### <a name="azure-firewall-recommendation-added-preview"></a>Azure Güvenlik Duvarı önerisi eklendi (Önizleme)

Tüm sanal ağlarınızı Azure Güvenlik Duvarı ile korumak için yeni bir öneri eklenmiştir.

Bu öneri, **sanal ağların Azure Güvenlik Duvarı tarafından korunması gerekir** ve Azure Güvenlik Duvarı 'nı kullanarak sanal ağlarınıza erişimi kısıtlayıp olası tehditleri önlemenizi önerir.

[Azure Güvenlik Duvarı](https://azure.microsoft.com/services/azure-firewall/)hakkında daha fazla bilgi edinin.


### <a name="authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix"></a>Yetkili IP aralıklarının, hızlı düzeltmeyle güncelleştirilmiş Kubernetes Hizmetleri önerisi üzerinde tanımlanması gerekir

Öneri **yetkılı IP aralıklarının artık, Kubernetes hizmetlerinde tanımlanması gerekir** . Şimdi hızlı bir çözüm seçeneği vardır.

Bu öneriye ve diğer tüm güvenlik merkezi önerilerine ilişkin daha fazla bilgi için bkz. [güvenlik önerileri-bir başvuru kılavuzu](recommendations-reference.md).

:::image type="content" source="./media/release-notes/authorized-ip-ranges-recommendation.png" alt-text="Yetkili IP aralıkları hızlı düzelme seçeneği ile Kubernetes Hizmetleri önerisi üzerinde tanımlanmalıdır":::


### <a name="regulatory-compliance-dashboard-now-includes-option-to-remove-standards"></a>Mevzuat uyumluluk panosu artık standartları kaldırma seçeneğini içermektedir

Güvenlik Merkezi 'nin yasal uyumluluk panosu, uyumlu uyumluluk denetimleri ve gereksinimlerini nasıl karşıladığınızı temel alarak uyumlulukla ilgili öngörüler sağlar.

Pano, varsayılan bir mevzuat standartları kümesi içerir. Sağlanan standartlardan herhangi biri kuruluşunuzla ilgili değilse, bu, yalnızca bir abonelik için kullanıcı arabiriminden kaldırmak üzere basit bir işlemdir. Standartlar yalnızca *abonelik* düzeyinde kaldırılabilir; Yönetim grubu kapsamı değil.

[Panonuzdan bir standart kaldırma](update-regulatory-compliance-packages.md#removing-a-standard-from-your-dashboard)hakkında daha fazla bilgi edinin.


### <a name="microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg"></a>Microsoft. Security/Securitydurumlarının tablosu Azure Kaynak grafiğinden kaldırıldı (ARG)

Azure Kaynak Grafiği, ortamınızı etkili bir şekilde yönetebilmeniz için belirli bir abonelik kümesi genelinde ölçeği sorgulama özelliği ile verimli kaynak araştırması sağlamak üzere tasarlanan Azure hizmetidir. 

Azure Güvenlik Merkezi için bağımsız değişken ve [kusto sorgu dili (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/) kullanarak çok çeşitli güvenlik sonrası verileri sorgulayabilirsiniz. Örnek:

- Varlık envanteri kullanır (bağımsız değişken)
- [Multi-Factor Authentication (MFA) etkin olmayan hesapların nasıl tanımlanacağına](security-center-identity-access.md#identify-accounts-without-multi-factor-authentication-mfa-enabled) yönelik örnek bir bağımsız değişken sorgusu belgeliyoruz

Bağımsız değişken dahilinde sorgularda kullanabileceğiniz veri tabloları vardır.

:::image type="content" source="./media/release-notes/azure-resource-graph-tables.png" alt-text="Azure Kaynak Grafiği Gezgini ve kullanılabilir tablolar":::

> [!TIP]
> ARG belgesi, [Azure Kaynak Grafiği tablosu ve kaynak türü başvurusu](../governance/resource-graph/reference/supported-tables-resources.md)'ndaki tüm kullanılabilir tabloları listeler.

Bu güncelleştirmeden, **Microsoft. Security/Securitydurumlarının** tablosu kaldırılmıştır. Securitydurumlarının API 'SI hala kullanılabilir.

Veri değiştirme, Microsoft. Security/değerlendirmeleri tablosu tarafından kullanılabilir.

Microsoft. Security/Securitydurumlardan ve Microsoft. Security/değerlendirmeleri arasındaki önemli fark, ilki değerlendirmelerin toplanmasının, saniyenin her biri için tek bir kayıt tuttuğunda olduğu sürece.

Örneğin, Microsoft. Security/Securitydurumlar, iki Polimanlar dizisiyle bir sonuç döndürür:

```
{
id: "/subscriptions/449bcidd-3470-4804-ab56-2752595 felab/resourceGroups/mico-rg/providers/Microsoft.Network/virtualNetworks/mico-rg-vnet/providers/Microsoft.Security/securityStatuses/mico-rg-vnet",
name: "mico-rg-vnet",
type: "Microsoft.Security/securityStatuses",
properties:  {
    policyAssessments: [
        {assessmentKey: "e3deicce-f4dd-3b34-e496-8b5381bazd7e", category: "Networking", policyName: "Azure DDOS Protection Standard should be enabled",...},
        {assessmentKey: "sefac66a-1ec5-b063-a824-eb28671dc527", category: "Compute", policyName: "",...}
    ],
    securitystateByCategory: [{category: "Networking", securityState: "None" }, {category: "Compute",...],
    name: "GenericResourceHealthProperties",
    type: "VirtualNetwork",
    securitystate: "High"
}
```
Ancak, Microsoft. Security/değerlendirmeleri, bu tür bir ilke değerlendirmesi için aşağıdaki gibi bir kayıt tutacaktır:

```
{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft. Network/virtualNetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/e3delcce-f4dd-3b34-e496-8b5381ba2d70",
name: "e3deicce-f4dd-3b34-e496-8b5381ba2d70",
properties:  {
    resourceDetails: {Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft.Network/virtualNetworks/mico-rg-vnet"...},
    displayName: "Azure DDOS Protection Standard should be enabled",
    status: (code: "NotApplicable", cause: "VnetHasNOAppGateways", description: "There are no Application Gateway resources attached to this Virtual Network"...}
}

{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/80fac66a-1ec5-be63-a824-eb28671dc527",
name: "8efac66a-1ec5-be63-a824-eb28671dc527",
properties: {
    resourceDetails: (Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet"...),
    displayName: "Audit diagnostic setting",
    status:  {code: "Unhealthy"}
}
```

**Artık değerlendirme tablosunu kullanmak için Securitydurumlar kullanarak var olan bir bağımsız değişken sorgusunun dönüştürülmesiyle bir örnek:**

Securitydurumlarının başvurduğu sorgu:

```kusto
SecurityResources 
| where type == 'microsoft.security/securitystatuses' and properties.type == 'virtualMachine'
| where name in ({vmnames}) 
| project name, resourceGroup, policyAssesments = properties.policyAssessments, resourceRegion = location, id, resourceDetails = properties.resourceDetails
```

Değerlendirme tablosu için değiştirme sorgusu:

```kusto
securityresources
| where type == "microsoft.security/assessments" and id contains "virtualMachine"
| extend resourceName = extract(@"(?i)/([^/]*)/providers/Microsoft.Security/assessments", 1, id)
| extend source = tostring(properties.resourceDetails.Source)
| extend resourceId = trim(" ", tolower(tostring(case(source =~ "azure", properties.resourceDetails.Id,
source =~ "aws", properties.additionalData.AzureResourceId,
source =~ "gcp", properties.additionalData.AzureResourceId,
extract("^(.+)/providers/Microsoft.Security/assessments/.+$",1,id)))))
| extend resourceGroup = tolower(tostring(split(resourceId, "/")[4]))
| where resourceName in ({vmnames}) 
| project resourceName, resourceGroup, resourceRegion = location, id, resourceDetails = properties.additionalData
```

Aşağıdaki bağlantılardan daha fazla bilgi edinin:
- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)
- [Kusto Sorgu Dili (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/)


## <a name="september-2020"></a>Eylül 2020

Eylül ayında güncelleştirmeler şunları içerir:
- [Güvenlik Merkezi yeni bir görünüm alır!](#security-center-gets-a-new-look)
- [Azure Defender yayımlandı](#azure-defender-released)
- [Key Vault için Azure Defender genel kullanıma sunuldu](#azure-defender-for-key-vault-is-generally-available)
- [Dosyalar ve ADLS 2. için depolama koruması için Azure Defender, genel kullanıma sunuldu](#azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available)
- [Varlık Envanteri araçları genel kullanıma sunuldu](#asset-inventory-tools-are-now-generally-available)
- [Kapsayıcı kayıt defterlerinin ve sanal makinelerin taranmasını bulma için belirli bir güvenlik açığını devre dışı bırakma](#disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines)
- [Kaynağı öneriden muaf tutma](#exempt-a-resource-from-a-recommendation)
- [Güvenlik Merkezi 'ndeki AWS ve GCP bağlayıcıları, çok bulut deneyimi getirir](#aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience)
- [Kubernetes iş yükü koruma önerisi paketi](#kubernetes-workload-protection-recommendation-bundle)
- [Güvenlik açığı değerlendirmesi bulguları artık sürekli dışarı aktarma için kullanılabilir](#vulnerability-assessment-findings-are-now-available-in-continuous-export)
- [Yeni kaynaklar oluştururken öneriler zorlanarak güvenlik yapılandırması yapılandırmalarını engelleyin](#prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources)
- [Ağ güvenlik grubu önerileri geliştirildi](#network-security-group-recommendations-improved)
- [Kullanım dışı önizleme AKS önerisi "Pod güvenlik Ilkeleri Kubernetes hizmetlerinde tanımlanmalıdır"](#deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services)
- [Azure Güvenlik Merkezi 'ndeki e-posta bildirimleri geliştirildi](#email-notifications-from-azure-security-center-improved)
- [Güvenli puan önizleme önerilerini içermez](#secure-score-doesnt-include-preview-recommendations)
- [Öneriler artık önem göstergesi ve yenilik aralığı içeriyor](#recommendations-now-include-a-severity-indicator-and-the-freshness-interval)


### <a name="security-center-gets-a-new-look"></a>Güvenlik Merkezi yeni bir görünüm alır!

Güvenlik Merkezi 'nin portal sayfaları için yenilenmiş bir kullanıcı arabirimi yayımladık. Yeni sayfalar, güvenli puan, varlık envanteri ve Azure Defender için panoları ve yeni bir genel bakış sayfası içerir.

Yeniden tasarlanan genel bakış sayfasında artık güvenli puan, varlık envanteri ve Azure Defender panolarına erişim için bir kutucuk vardır. Ayrıca, mevzuat uyumluluk panosuna bağlanan bir kutucuk vardır.

[Genel bakış sayfası](overview-page.md)hakkında daha fazla bilgi edinin.


### <a name="azure-defender-released"></a>Azure Defender yayımlandı

**Azure Defender** , Azure ve hibrit iş yüklerinizin gelişmiş, akıllı ve koruma Için Güvenlik Merkezi kapsamında tümleşik olan bulut iş yükü koruma platformudur (cwpp). Güvenlik Merkezi 'nin standart fiyatlandırma katmanı seçeneğini değiştirir. 

Azure Defender 'ı Azure Güvenlik Merkezi 'nin **fiyatlandırma ve ayarlar** alanından etkinleştirdiğinizde, aşağıdaki Defender planlarının hepsi aynı anda etkinleştirilmiştir ve ortamınızın işlem, veri ve hizmet katmanları için kapsamlı savunma sağlar:

- [Sunucular için Azure Defender](defender-for-servers-introduction.md)
- [App Service için Azure Defender](defender-for-app-service-introduction.md)
- [Depolama için Azure Defender](defender-for-storage-introduction.md)
- [SQL için Azure Defender](defender-for-sql-introduction.md)
- [Key Vault için Azure Defender](defender-for-key-vault-introduction.md)
- [Kubernetes için Azure Defender](defender-for-kubernetes-introduction.md)
- [Kapsayıcı kayıt defterleri için Azure Defender](defender-for-container-registries-introduction.md)

Bu planların her biri, güvenlik merkezi belgelerinde ayrı olarak açıklanmıştır.

Adanmış panosu sayesinde Azure Defender, sanal makineler, SQL veritabanları, kapsayıcılar, Web uygulamaları, ağınız ve daha fazlası için güvenlik uyarıları ve Gelişmiş tehdit koruması sağlar.

[Azure Defender hakkında daha fazla bilgi](azure-defender.md)

### <a name="azure-defender-for-key-vault-is-generally-available"></a>Key Vault için Azure Defender genel kullanıma sunuldu

Azure Key Vault, şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi gizli dizileri koruyan bir bulut hizmetidir. 

**Key Vault Için Azure Defender** , ek bir güvenlik zekası katmanı sunarak Azure Key Vault için Azure yerel ve Gelişmiş tehdit koruması sağlar. Uzantıya göre Key Vault için Azure Defender, bu nedenle Key Vault hesaplarınıza bağlı kaynakların çoğunu koruyor.

İsteğe bağlı plan artık GA 'dir. Bu özellik, "Azure Key Vault için Gelişmiş tehdit koruması" olarak önizlemededir.

Ayrıca, Azure portal Key Vault sayfalarında artık **Güvenlik Merkezi** önerileri ve uyarıları için ayrılmış bir **güvenlik** sayfası vardır.

[Key Vault Için Azure Defender](defender-for-key-vault-introduction.md)'da daha fazla bilgi edinin.


### <a name="azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available"></a>Dosyalar ve ADLS 2. için depolama koruması için Azure Defender, genel kullanıma sunuldu 

**Depolama Için Azure Defender** , Azure depolama hesaplarınızdaki zararlı olabilecek etkinlikleri algılar. Verileriniz, blob kapsayıcıları, dosya paylaşımları veya veri Lakes olarak depolanıp saklanmadığı için korunabilir.

[Azure dosyaları](../storage/files/storage-files-introduction.md) ve [Azure Data Lake Storage 2.](../storage/blobs/data-lake-storage-introduction.md) için destek genel kullanıma sunulmuştur.

1 Ekim 2020 ' den bu hizmetlerde kaynakları koruma ücretlendirmeye başlayacağız.

[Depolama Için Azure Defender](defender-for-storage-introduction.md)'da daha fazla bilgi edinin.


### <a name="asset-inventory-tools-are-now-generally-available"></a>Varlık Envanteri araçları genel kullanıma sunuldu

Azure Güvenlik Merkezi 'nin varlık Envanteri sayfası, güvenlik merkezi 'ne bağladığınız kaynakların güvenlik duruşunu görüntülemek için tek bir sayfa sağlar.

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bu güvenlik açıklarını nasıl düzeltebileceğiniz konusunda öneriler sağlar.

Herhangi bir kaynakta bekleyen öneriler olduğunda, bu değişiklikler envanterde görüntülenir.

[Varlık envanteri ve yönetim araçlarıyla kaynaklarınızın araştırıp yönetiminde](asset-inventory.md)daha fazla bilgi edinin.



### <a name="disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines"></a>Kapsayıcı kayıt defterlerinin ve sanal makinelerin taranmasını bulma için belirli bir güvenlik açığını devre dışı bırakma

Azure Defender, Azure Container Registry ve sanal makinelerinizdeki görüntüleri taramak için güvenlik açığı tarayıcıları içerir.

Bir bulmayı yok saymanız gereken bir kuruluş varsa, bunu düzeltmek yerine isteğe bağlı olarak devre dışı bırakabilirsiniz. Devre dışı bulgular, güvenli puanınızı etkilemez veya istenmeyen gürültü oluşturmaz.

Bir bulma, devre dışı bırakma kurallarında tanımladığınız ölçütlerle eşleştiğinde, bu, bulguları listesinde görünmez.

Bu seçenek, için öneriler ayrıntıları sayfalarından kullanılabilir:

- **Azure Container Registry görüntülerdeki güvenlik açıkları düzeltilmelidir**
- **Sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir**

[Kapsayıcı görüntüleriniz için belirli bulguları devre dışı bırakma konusunda](defender-for-container-registries-usage.md#disable-specific-findings-preview) daha fazla bilgi edinin ve [sanal makineleriniz için belirli bulguları devre dışı bırakın](remediate-vulnerability-findings-vm.md#disable-specific-findings-preview)


### <a name="exempt-a-resource-from-a-recommendation"></a>Kaynağı öneriden muaf tutma

Bazen bir kaynak belirli bir öneriyle ilgili sağlıksız olarak listelenecektir (ve bu nedenle, güvenli puanınızı düşürürken). Güvenlik Merkezi tarafından izlenmeyen bir işlem tarafından düzeltilebilir olabilir. Ya da kuruluşunuz söz konusu kaynak için riski kabul etmeye karar verdi. 

Bu gibi durumlarda, bir istisna kuralı oluşturabilir ve ileride kaynağın sağlıksız kaynaklar arasında listelenmemesini sağlayabilirsiniz. Bu kurallar, aşağıda açıklandığı gibi belgelenmiş gerekçeler içerebilir.

[Bir kaynağı önerilerden ve güvenli puanın dışında bırakma](exempt-resource.md)hakkında daha fazla bilgi edinin.


### <a name="aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience"></a>Güvenlik Merkezi 'ndeki AWS ve GCP bağlayıcıları, çok bulut deneyimi getirir

Bulut güvenlik hizmetleri genellikle birden çok bulut platformunu kapsayan bulut iş yükleri ile aynı olmalıdır.

Azure Güvenlik Merkezi artık Azure, Amazon Web Services (AWS) ve Google Cloud Platform (GCP) iş yüklerini korur.

AWS ve GCP hesaplarınızı Güvenlik Merkezi 'ne ekleme, AWS güvenlik hub, GCP güvenlik komutu ve Azure Güvenlik Merkezi ' ni tümleştirir. 

[AWS hesaplarınızı Azure Güvenlik Merkezi 'Ne bağlama](quickstart-onboard-aws.md) ve [GCP hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama](quickstart-onboard-gcp.md)konusunda daha fazla bilgi edinin.


### <a name="kubernetes-workload-protection-recommendation-bundle"></a>Kubernetes iş yükü koruma önerisi paketi

Kubernetes iş yüklerinin güvenli olduğundan emin olmak için, Güvenlik Merkezi, Kubernetes giriş denetimi ile zorlama seçenekleri dahil olmak üzere Kubernetes düzeyi sağlamlaştırma önerilerini ekliyor.

AKS kümenizdeki Kubernetes için Azure Ilkesi eklentisini yüklediğinizde, Kubernetes API sunucusuna gönderilen her istek, kümede kalıcı hale gelmeden önce önceden tanımlanmış en iyi uygulamalar kümesine göre izlenir. Daha sonra, en iyi uygulamaları zorlamak ve gelecekteki iş yükleri için bunları zorunlu kılmak üzere yapılandırabilirsiniz.

Örneğin, ayrıcalıklı kapsayıcıların oluşturulmaması ve ileride yapılacak istekleri engellenemeyeceksiniz.

[Kubernetes giriş denetimini kullanan Iş yükü koruma en iyi uygulamaları](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)hakkında daha fazla bilgi edinin.


### <a name="vulnerability-assessment-findings-are-now-available-in-continuous-export"></a>Güvenlik açığı değerlendirmesi bulguları artık sürekli dışarı aktarma için kullanılabilir

Azure Event Hubs, Log Analytics çalışma alanları veya Azure Izleyici ile uyarı ve önerilerinizi gerçek zamanlı olarak akışa almak için sürekli dışarı aktarmayı kullanın. Buradan, bu verileri Sıems ile tümleştirebilirsiniz (Azure Sentinel, Power BI, Azure Veri Gezgini ve daha fazlasını yapabilirsiniz.

Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme araçları, "ana" önerilerinde "sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir" gibi eyleme dönüştürülebilir öneriler olarak kaynaklarınızın bulgularını döndürür. 

Öneriler ' i seçip **güvenlik bulgularını dahil et** seçeneğini etkinleştirdiğinizde, güvenlik bulguları sürekli dışarı aktarma aracılığıyla dışarı aktarmaya hazırdır.

:::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="Sürekli dışa aktarma yapılandırmasında güvenlik bulgularını dahil et" :::

İlgili sayfalar:

- [Azure sanal makineleri için Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme çözümü](deploy-vulnerability-assessment-vm.md)
- [Azure Container Registry görüntüleri için Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme çözümü](defender-for-container-registries-usage.md)
- [Sürekli dışarı aktarma](continuous-export.md)

### <a name="prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources"></a>Yeni kaynaklar oluştururken öneriler zorlanarak güvenlik yapılandırması yapılandırmalarını engelleyin

Güvenlik yapılandırması hataları, güvenlik olaylarının önemli bir nedendir. Güvenlik Merkezi artık, belirli önerilere göre yeni kaynakların yanlış yapılandırmalarını *önlemeye* yardımcı olur. 

Bu özellik, iş yüklerinizi güvende tutmaya ve güvenli puanınızı sabitetmenize yardımcı olabilir.

Belirli bir öneriye göre güvenli bir yapılandırma uygulamak, iki modda sunulur:

- Azure Ilkesinin **reddetme** efektini kullanarak, sağlıksız kaynakların oluşturulmasını durdurabilirsiniz

- **Uygula** seçeneğini kullanarak Azure Ilkesinin **Deployifnotexist** efektinin avantajlarından yararlanabilir ve oluşturma sırasında uyumlu olmayan kaynakları otomatik olarak düzeltebilirsiniz
 
Bu, seçilen güvenlik önerileri için kullanılabilir ve kaynak Ayrıntıları sayfasının en üstünde bulunabilir.

[Zorla/reddetme önerilerini kullanarak yanlış yapılandırma önleme konusunda](prevent-misconfigurations.md)daha fazla bilgi edinin.

###  <a name="network-security-group-recommendations-improved"></a>Ağ güvenlik grubu önerileri geliştirildi

Ağ güvenlik gruplarıyla ilgili aşağıdaki güvenlik önerileri, bazı hatalı pozitif sonuç örneklerini azaltmak için geliştirilmiştir.

- Tüm ağ bağlantı noktaları, VM 'niz ile ilişkili NSG 'de sınırlandırılmalıdır
- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır
- Internet 'e yönelik sanal makineler ağ güvenlik gruplarıyla korunmalıdır
- Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir


### <a name="deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services"></a>Kullanım dışı önizleme AKS önerisi "Pod güvenlik Ilkeleri Kubernetes hizmetlerinde tanımlanmalıdır"

Önizleme önerisi "Pod güvenlik Ilkeleri, Kubernetes hizmetlerinde tanımlanmalıdır", [Azure Kubernetes hizmeti](../aks/use-pod-security-policies.md) belgelerinde açıklandığı şekilde kullanımdan kaldırılmıştır.

Pod güvenlik ilkesi (Önizleme) özelliği kullanımdan kaldırma için ayarlanmış ve 15 Ekim 2020 ' den sonra AKS için Azure Ilkesi 'nin kullanım dışı bırakılmayacak.

Pod güvenlik ilkesi (Önizleme) kullanım dışı olduktan sonra, gelecekteki küme yükseltmelerini gerçekleştirmek ve Azure desteği içinde kalmak için kullanımdan kaldırılan özelliği kullanarak mevcut kümelerin özelliğini devre dışı bırakmanız gerekir.


### <a name="email-notifications-from-azure-security-center-improved"></a>Azure Güvenlik Merkezi 'ndeki e-posta bildirimleri geliştirildi

Güvenlik uyarıları ile ilgili e-postaların aşağıdaki bölgeleri geliştirilmiştir: 

- Tüm önem düzeyleri için uyarılar hakkında e-posta bildirimleri gönderme özelliği eklendi
- Kullanıcılara abonelikte farklı RBAC rolleri bildirme yeteneği eklendi
- En yüksek öneme sahip uyarılarda (orijinal ihlal etmeme olasılığı yüksek olan), abonelik sahiplerini varsayılan olarak önceden bilgilendiriyoruz
- E-posta bildirimleri yapılandırma sayfasından telefon numarası alanını kaldırdık

[Güvenlik uyarıları için e-posta bildirimlerini ayarlama](security-center-provide-security-contact-details.md)bölümünde daha fazla bilgi edinin.


### <a name="secure-score-doesnt-include-preview-recommendations"></a>Güvenli puan önizleme önerilerini içermez 

Güvenlik Merkezi, güvenlik sorunları için kaynaklarınızı, aboneliklerinizi ve kuruluşunuzu sürekli olarak değerlendirir. Daha sonra her türlü bulguları tek bir puanın içinde toplar, böylece bir bakışta geçerli güvenlik durumunuza göre daha yüksek puan, belirtilen risk düzeyini azaltır.

Yeni tehditler bulunduğundan yeni güvenlik önerileri yeni öneriler aracılığıyla güvenlik merkezi 'nde kullanılabilir hale getirilir. Güvenli puanınızın beklenmedik şekilde değişiklik yaşamamasını önlemek ve yeni önerilerin puanlarını etkilemeden önce keşfedebileceğiniz bir yetkisiz kullanım süresi sağlamak için, **Önizleme** olarak işaretlenen öneriler artık güvenli puanınızın hesaplamalarına dahil değildir. Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunduklarında, bunlar mümkün olduğunda yine de düzeltilmelidir.

Ayrıca, **Önizleme** önerileri bir kaynağı "sağlıksız" olarak işlemez.

Önizleme önerisi örneği:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Önizleme bayrağıyla ilgili öneri":::

[Güvenli skor hakkında daha fazla bilgi edinin](secure-score-security-controls.md).


### <a name="recommendations-now-include-a-severity-indicator-and-the-freshness-interval"></a>Öneriler artık önem göstergesi ve yenilik aralığı içeriyor

Önerilerin ayrıntılar sayfasında artık bir yeniliği aralığı göstergesi (her ne zaman geçerlidir) ve önerinin önem derecesine ilişkin net bir ekran görüntülenir.

:::image type="content" source="./media/release-notes/recommendations-severity-freshness-indicators.png" alt-text="Yeniliği ve önem derecesini gösteren öneri sayfası":::



## <a name="august-2020"></a>Ağustos 2020

Ağustos ayında güncelleştirmeler şunlardır:

- [Varlık envanteri-varlıklarınızın güvenlik sonrası yeni görünümü](#asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets)
- [Azure Active Directory güvenlik varsayılanları için destek eklendi (Multi-Factor Authentication için)](#added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication)
- [Hizmet sorumlusu önerisi eklendi](#service-principals-recommendation-added)
- [VM 'lerde güvenlik açığı değerlendirmesi-birleştirilmiş öneriler ve ilkeler](#vulnerability-assessment-on-vms---recommendations-and-policies-consolidated)
- [ASC_default Initiative 'e eklenen yeni AKS güvenlik ilkeleri – yalnızca özel önizleme müşterileri tarafından kullanılmak üzere](#new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only)


### <a name="asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets"></a>Varlık envanteri-varlıklarınızın güvenlik sonrası yeni görünümü

Güvenlik Merkezi 'nin varlık Envanteri (Şu anda önizleme aşamasındadır), güvenlik merkezi 'ne bağladığınız kaynakların güvenlik duruşunu görüntülemek için bir yol sağlar.

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bu güvenlik açıklarını nasıl düzeltebileceğiniz konusunda öneriler sağlar. Herhangi bir kaynakta bekleyen öneriler olduğunda, bu değişiklikler envanterde görüntülenir.

Güvenlik sonrası verilerinizi araştırmak ve bulgularınızı temel alarak daha fazla eylem gerçekleştirmek için görünümü ve filtrelerini kullanabilirsiniz.

[Varlık envanteri](asset-inventory.md)hakkında daha fazla bilgi edinin.


### <a name="added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication"></a>Azure Active Directory güvenlik varsayılanları için destek eklendi (Multi-Factor Authentication için)

Güvenlik Merkezi, Microsoft 'un ücretsiz kimlik güvenlik korumalarının [güvenlik Varsayılanları](../active-directory/fundamentals/concept-fundamentals-security-defaults.md)için tam destek ekledi.

Güvenlik Varsayılanları, kuruluşunuzun kimlik ile ilgili ortak saldırılardan savunmak için önceden yapılandırılmış kimlik güvenlik ayarlarını sağlar. Güvenlik Varsayılanları, genel olarak 5.000.000 taneden fazla kiracı koruuyor; 50.000 kiracılar da Güvenlik Merkezi tarafından korunur.

Güvenlik Merkezi artık güvenlik Varsayılanları etkinleştirilmeden bir Azure aboneliğini her belirlediğinde bir güvenlik önerisi sunmaktadır. Bu aşamada, Güvenlik Merkezi, Azure Active Directory (AD) Premium lisansının bir parçası olan koşullu erişimi kullanarak Multi-Factor Authentication 'ı etkinleştirmeyi öneririz. Azure AD Ücretsiz kullanan müşteriler için artık güvenlik varsayılanlarını etkinleştirmenizi öneririz. 

Amacınız, daha fazla müşteriyi MFA ile bulut ortamlarını güvenli hale getirmeye teşvik etmek ve ayrıca, [güvenli puanınızın](secure-score-security-controls.md)en yüksek risklerinden birini hafifletmektir.

[Güvenlik Varsayılanları](../active-directory/fundamentals/concept-fundamentals-security-defaults.md)hakkında daha fazla bilgi edinin.


### <a name="service-principals-recommendation-added"></a>Hizmet sorumlusu önerisi eklendi

Güvenlik Merkezi müşterilerinin aboneliklerini yönetmek için yönetim sertifikaları kullanmalarını sağlamak üzere yeni bir öneri eklenmiştir.

Kullanım önerisi, **yönetim sertifikaları yerine aboneliklerinizi korumak Için hizmet sorumlularını** , aboneliklerinizi daha güvenli bir şekilde yönetmek Için hizmet sorumlularını veya Azure Resource Manager kullanmanızı önerir. 

[Azure Active Directory Içindeki uygulama ve hizmet sorumlusu nesneleri](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object)hakkında daha fazla bilgi edinin.


### <a name="vulnerability-assessment-on-vms---recommendations-and-policies-consolidated"></a>VM 'lerde güvenlik açığı değerlendirmesi-birleştirilmiş öneriler ve ilkeler

Güvenlik Merkezi, bir güvenlik açığı değerlendirme çözümü çalıştırıp çalıştırmadığını belirlemek için sanal makinelerinizi inceler. Bir güvenlik açığı değerlendirme çözümü bulunmazsa, güvenlik merkezi dağıtımı basitleştirmeye yönelik bir öneri sunar.

Güvenlik açıkları bulunduğunda Güvenlik Merkezi, gerektiğinde araştırmanız ve düzeltilmesi için bulguları özetlemeye yönelik bir öneri sunar.

Kullandıkları tarayıcı türünden bağımsız olarak tüm kullanıcılar için tutarlı bir deneyim sağlamak amacıyla, aşağıdaki iki öneriyle dört öneri sunuyoruz:

|Birleşik öneri|Açıklamayı Değiştir|
|----|:----|
|**Sanal makinelerinizde bir güvenlik açığı değerlendirme çözümünün etkinleştirilmesi gerekir**|Aşağıdaki iki öneriyi değiştirir:<br> **•** Sanal makinelerde yerleşik güvenlik açığı değerlendirme çözümünü etkinleştirin (Qualys (artık kullanım dışı) ile desteklenir (Standart katmana dahildir)<br> **•** Güvenlik açığı değerlendirme çözümünün sanal makinelerinizde yüklü olması gerekir (artık kullanım dışı) (Standart ve ücretsiz Katmanlar)|
|**Sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir**|Aşağıdaki iki öneriyi değiştirir:<br>**•** Sanal makinelerinizde bulunan güvenlik açıklarını düzeltin (Qualys tarafından desteklenir) (artık kullanım dışı)<br>**•** Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir (artık kullanım dışı)|
|||

Artık güvenlik merkezi 'nin güvenlik açığı değerlendirmesi uzantısını veya özel olarak lisanslı bir çözümü ("KLG") Qualys veya Rapid7 gibi bir ortaktan dağıtmak için aynı öneriyi kullanacaksınız.

Ayrıca, güvenlik açıkları bulunduğunda ve Güvenlik Merkezi 'ne bildirildiğine göre tek bir öneri, bunları tanımlayan güvenlik açığı değerlendirme çözümüne bakılmaksızın sizi bulgulara bildirir.

#### <a name="updating-dependencies"></a>Bağımlılıklar güncelleştiriliyor

Önceki önerilere veya ilke anahtarlarına/adlara başvuruda bulunan komut dosyalarınız, sorgular veya otomatiklamalar varsa, başvuruları güncelleştirmek için aşağıdaki tabloları kullanın:

##### <a name="before-august-2020"></a>Ağustos 2020 öncesi

|Öneri|Kapsam|
|----|:----|
|**Sanal makinelerde yerleşik güvenlik açığı değerlendirme çözümünü etkinleştirin (Qualys tarafından desteklenir)**<br>Anahtar: 550e890b-e652-4d22-8274-60b3bdb24c63|Yerleşik|
|**Sanal makinelerinizde bulunan güvenlik açıklarını düzeltin (Qualys tarafından desteklenir)**<br>Anahtar: 1195aff-c881-495E-9bc5-1486211ae03f|Yerleşik|
|**Güvenlik açığı değerlendirme çözümünün sanal makinelerinizde yüklü olması gerekir**<br>Anahtar: 01b1ed4c-B733-4FEE-b145-f23236e70cf3|KLG|
|**Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir**<br>Anahtar: 71992a2a-d168-42e0-b10e-6b45fa2ecddb|KLG|
||||


|İlke|Kapsam|
|----|:----|
|**Güvenlik açığı değerlendirmesi sanal makinelerde etkinleştirilmelidir**<br>İlke KIMLIĞI: 501541f7-f7e7-4cd6-868C-4190fdad3ac9|Yerleşik|
|**Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir**<br>İlke KIMLIĞI: 760a85ff-6162-42b3-8d70-698e268f648c|KLG|
||||


##### <a name="from-august-2020"></a>Ağustos 2020 ' den

|Öneri|Kapsam|
|----|:----|
|**Sanal makinelerinizde bir güvenlik açığı değerlendirme çözümünün etkinleştirilmesi gerekir**<br>Anahtar: ffff0522-1e88-47fc-8382-2a80ba848f5d|Yerleşik + KLG|
|**Sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir**<br>Anahtar: 1195aff-c881-495E-9bc5-1486211ae03f|Yerleşik + KLG|
||||

|İlke|Kapsam|
|----|:----|
|[**Güvenlik açığı değerlendirmesi sanal makinelerde etkinleştirilmelidir**](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f501541f7-f7e7-4cd6-868c-4190fdad3ac9)<br>İlke KIMLIĞI: 501541f7-f7e7-4cd6-868C-4190fdad3ac9 |Yerleşik + KLG|
||||


### <a name="new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only"></a>ASC_default Initiative 'e eklenen yeni AKS güvenlik ilkeleri – yalnızca özel önizleme müşterileri tarafından kullanılmak üzere

Kubernetes iş yüklerinin varsayılan olarak güvenli olduğundan emin olmak için, güvenlik merkezi Kubernetes, Kubernetes giriş denetimiyle zorlama seçenekleri de dahil olmak üzere Kubernetes düzey ilkelerini ve sağlamlaştırma

Bu projenin erken aşaması, özel bir önizleme ve ASC_default girişiminin yeni (varsayılan olarak devre dışı) ilkelerinin eklenmesini içerir.

Bu ilkeleri güvenle yoksayabilirsiniz, ortamınızda hiçbir etkisi olmaz. Bunları etkinleştirmek istiyorsanız, önizleme için kaydolun https://aka.ms/SecurityPrP ve aşağıdaki seçeneklerden seçim yapın:

1. **Tek önizleme** – yalnızca bu özel önizlemeye katılacak şekilde. Birleştirmek istediğiniz önizleme olarak "ASC sürekli tarama" yı kesin olarak belirtmeyi unutmayın.
1. **Devam eden program** – bu ve gelecekteki özel önizlemelere eklenmek üzere. Bir profil ve gizlilik sözleşmesini doldurmanız gerekir.


## <a name="july-2020"></a>Temmuz 2020

Temmuz ayında güncelleştirmeler şunları içerir:
- [Sanal makineler için güvenlik açığı değerlendirmesi artık Market olmayan görüntüler için kullanılabilir](#vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images)
- [Azure depolama için tehdit koruması Azure dosyaları ve Azure Data Lake Storage 2. içerecek şekilde genişletildi (Önizleme)](#threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview)
- [Tehdit koruması özelliklerini etkinleştirmeye yönelik sekiz yeni öneri](#eight-new-recommendations-to-enable-threat-protection-features)
- [Kapsayıcı güvenlik geliştirmeleri-daha hızlı kayıt defteri tarama ve yenilenen belgeler](#container-security-improvements---faster-registry-scanning-and-refreshed-documentation)
- [Uyarlamalı uygulama denetimleri, yol kurallarında joker karakterlere yönelik yeni bir öneri ve destek ile güncelleştirildi](#adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules)
- [SQL gelişmiş veri güvenliği için altı ilke kullanım dışı](#six-policies-for-sql-advanced-data-security-deprecated)




### <a name="vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images"></a>Sanal makineler için güvenlik açığı değerlendirmesi artık Market olmayan görüntüler için kullanılabilir

Güvenlik Merkezi bir güvenlik açığı değerlendirme çözümü dağıttığınızda, dağıtım öncesinde daha önce bir doğrulama denetimi gerçekleştirdi. Denetim, hedef sanal makinenin Market SKU 'sunu doğrulamamıştı. 

Bu güncelleştirmeden denetim kaldırılmıştır ve artık ' özel ' Windows ve Linux makinelerine güvenlik açığı değerlendirme araçları dağıtabilirsiniz. Özel görüntüler, Market varsayılanlarından değiştirdiğiniz alanlardır.

Artık tümleşik güvenlik açığı değerlendirme uzantısını (Qualys tarafından desteklenen) çok sayıda makineye dağıtabseniz de, destek yalnızca [Tümleşik güvenlik açığı tarayıcısını Standart katman VM 'Lerine dağıtma](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines) bölümünde listelenen bir işletim sistemi kullanıyorsanız kullanılabilir

[Sanal makineler için tümleşik güvenlik açığı tarayıcısı hakkında daha fazla bilgi edinin (Azure Defender gerekir)](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner).

Özel olarak lisanslanmış güvenlik açığı değerlendirme çözümünüzü [bir iş ortağı güvenlik açığı tarama çözümü dağıtma](deploy-vulnerability-assessment-vm.md)konusunda Qualys veya Rapid7 adresinden kullanma hakkında daha fazla bilgi edinin.


### <a name="threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview"></a>Azure depolama için tehdit koruması Azure dosyaları ve Azure Data Lake Storage 2. içerecek şekilde genişletildi (Önizleme)

Azure depolama için tehdit koruması, Azure depolama hesaplarınızdaki zararlı olabilecek etkinlikleri algılar. Güvenlik Merkezi, depolama hesaplarınıza erişme veya bu hesapları kullanma girişimlerini algıladığında uyarıları görüntüler. 

Verileriniz, blob kapsayıcıları, dosya paylaşımları veya veri Lakes olarak depolanıp saklanmadığı için korunabilir.




### <a name="eight-new-recommendations-to-enable-threat-protection-features"></a>Tehdit koruması özelliklerini etkinleştirmeye yönelik sekiz yeni öneri

Azure Güvenlik Merkezi 'nin tehdit koruması özelliklerinin aşağıdaki kaynak türleri için etkinleştirilmesi için basit bir yol sağlamak üzere sekiz yeni öneri eklenmiştir: sanal makineler, App Service planlar, Azure SQL veritabanı sunucuları, makinelerdeki SQL Server 'lar, Azure depolama hesapları, Azure Kubernetes hizmet kümeleri, Azure Container Registry kayıt defterleri ve Azure Key Vault kasaları.

Yeni öneriler şunlardır:

- **Azure SQL veritabanı sunucularında gelişmiş veri güvenliği etkinleştirilmelidir**
- **Makinelerdeki SQL sunucularında gelişmiş veri güvenliği etkinleştirilmelidir**
- **Gelişmiş tehdit koruması Azure App Service planlarda etkinleştirilmelidir**
- **Gelişmiş tehdit koruması Azure Container Registry kayıt defterlerinde etkinleştirilmelidir**
- **Gelişmiş tehdit koruması Azure Key Vault kasaları üzerinde etkinleştirilmelidir**
- **Gelişmiş tehdit koruması, Azure Kubernetes hizmet kümelerinde etkinleştirilmelidir**
- **Gelişmiş tehdit koruması, Azure depolama hesaplarında etkinleştirilmelidir**
- **Gelişmiş tehdit koruması sanal makinelerde etkinleştirilmelidir**

Bu yeni öneriler, **Azure Defender** güvenlik denetimini etkinleştir ' e aittir.

Öneriler de hızlı düzelme özelliğini içerir. 

> [!IMPORTANT]
> Bu önerilerin herhangi birini yeniden yapmak, ilgili kaynakların korunması için ücretlendirmesiyle sonuçlanır. Geçerli abonelikte ilgili kaynaklarınız varsa, bu ücretler hemen başlayacaktır. Daha sonraki bir tarihte daha sonra ekleyebilirsiniz.
> 
> Örneğin, aboneliğinizde hiç Azure Kubernetes hizmet kümeniz yoksa ve tehdit korumasını etkinleştirirseniz, hiçbir ücret alınmaz. Gelecekte aynı aboneliğe bir küme eklerseniz, bu, otomatik olarak korunur ve ücretler bu anda başlayacaktır.

[Güvenlik önerileri başvuru sayfasında](recommendations-reference.md)bunların her biri hakkında daha fazla bilgi edinin.

[Azure Güvenlik Merkezi 'nde tehdit koruması](azure-defender.md)hakkında daha fazla bilgi edinin.




### <a name="container-security-improvements---faster-registry-scanning-and-refreshed-documentation"></a>Kapsayıcı güvenlik geliştirmeleri-daha hızlı kayıt defteri tarama ve yenilenen belgeler

Kapsayıcı güvenlik etki alanındaki sürekli yatırımların bir parçası olarak, güvenlik merkezi 'nin Azure Container Registry ' de depolanan kapsayıcı görüntülerinin dinamik taramalarında önemli bir performans geliştirmesini paylaşabiliriz. Şimdi taramalar genellikle yaklaşık iki dakika içinde tamamlanır. Bazı durumlarda, bu işlem 15 dakikaya kadar sürebilir.

Azure Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri ile ilgili açıklık ve kılavuz kalitesini artırmak için kapsayıcı güvenliği belge sayfalarını de yeniledik. 

Aşağıdaki makalelerde Güvenlik Merkezi 'nin kapsayıcı güvenliği hakkında daha fazla bilgi edinin:

- [Güvenlik Merkezi 'nin kapsayıcı güvenlik özelliklerine genel bakış](container-security.md)
- [Azure Container Registry ile tümleştirmenin ayrıntıları](defender-for-container-registries-introduction.md)
- [Azure Kubernetes hizmeti ile tümleştirmenin ayrıntıları](defender-for-kubernetes-introduction.md)
- [Kayıt defterlerine nasıl tarama yapılır ve Docker konaklarınızı sağlamlaştırın](container-security.md)
- [Azure Kubernetes hizmet kümeleri için tehdit koruması özelliklerinden güvenlik uyarıları](alerts-reference.md#alerts-akscluster)
- [Azure Kubernetes hizmet konakları için tehdit koruması özelliklerinden güvenlik uyarıları](alerts-reference.md#alerts-containerhost)
- [Kapsayıcılar için güvenlik önerileri](recommendations-reference.md#recs-containers)



### <a name="adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules"></a>Uyarlamalı uygulama denetimleri, yol kurallarında joker karakterlere yönelik yeni bir öneri ve destek ile güncelleştirildi

Uyarlamalı uygulama denetimleri özelliği iki önemli güncelleştirme aldı:

* Yeni bir öneri, daha önce izin verilmeyen olası yasal davranışı tanımlar. **Uyarlamalı uygulama denetim ilkenizde yeni öneri, Allowlist kuralları güncellenmelidir** , uyarlamalı uygulama denetimleri ihlal uyarılarındaki hatalı pozitif sonuç sayısını azaltmak için mevcut ilkeye yeni kurallar eklemenizi ister.

* Yol kuralları artık joker karakterleri destekliyor. Bu güncelleştirmeden, joker karakterleri kullanarak izin verilen yol kurallarını yapılandırabilirsiniz. Desteklenen iki senaryo vardır:

    * Bu klasörde ve alt klasörlerde bulunan tüm yürütülebilir dosyalara izin vermek için yolun sonunda bir joker karakter kullanma

    * Bilinen bir yürütülebilir adı, değişen bir klasör adıyla (örn. bilinen bir yürütülebiliri olan kişisel Kullanıcı klasörleri, otomatik olarak oluşturulan klasör adları, vb.) etkinleştirmek için yolun ortasında bir joker karakter kullanma.


[Uyarlamalı uygulama denetimleri hakkında daha fazla bilgi edinin](security-center-adaptive-application.md).



### <a name="six-policies-for-sql-advanced-data-security-deprecated"></a>SQL gelişmiş veri güvenliği için altı ilke kullanım dışı

SQL makinelerinde gelişmiş veri güvenliği ile ilgili altı ilke kullanım dışı bırakılıyor:

- Gelişmiş tehdit koruması türleri SQL yönetilen örneği gelişmiş veri güvenliği ayarları içinde ' All ' olarak ayarlanmalıdır
- Gelişmiş tehdit koruması türleri SQL Server Gelişmiş veri güvenliği ayarları 'nda ' tümü ' olarak ayarlanmalıdır
- SQL yönetilen örneği için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir
- SQL Server için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir
- SQL yönetilen örnek gelişmiş veri güvenliği ayarları 'nda yöneticilere ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir
- SQL Server Gelişmiş veri güvenliği ayarları 'nda Yöneticiler ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir

[Yerleşik ilkeler](./policy-reference.md)hakkında daha fazla bilgi edinin.





## <a name="june-2020"></a>Haziran 2020

Haziran 'daki güncelleştirmeler şunlardır:
- [Güvenli puan API 'SI (Önizleme)](#secure-score-api-preview)
- [SQL makineler için gelişmiş veri güvenliği (Azure, diğer bulutlar ve şirket içi) (Önizleme)](#advanced-data-security-for-sql-machines-azure-other-clouds-and-on-prem-preview)
- [Log Analytics aracısını Azure Arc makinelerine dağıtmaya yönelik iki yeni öneri (Önizleme)](#two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview)
- [Ölçekte sürekli dışa aktarma ve iş akışı Otomasyonu yapılandırması oluşturmaya yönelik yeni ilkeler](#new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale)
- [İnternet 'e yönelik olmayan sanal makineleri korumak için NSG 'lerin kullanılmasına yönelik yeni öneri](#new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines)
- [Tehdit koruması ve gelişmiş veri güvenliğini etkinleştirmek için yeni ilkeler](#new-policies-for-enabling-threat-protection-and-advanced-data-security)



### <a name="secure-score-api-preview"></a>Güvenli puan API 'SI (Önizleme)

Artık puanınızı [güvenli Puanlama API 'si](/rest/api/securitycenter/securescores/) (Şu anda önizleme aşamasında) aracılığıyla erişebilirsiniz. API yöntemleri, verileri sorgulama ve zaman içinde güvenli Puanlarınızın kendi raporlama mekanizmanızı oluşturma esnekliğini sağlar. Örneğin, belirli bir aboneliğin Puanını almak için **güvenli puanlar** API 'sini kullanabilirsiniz. Ayrıca, güvenlik denetimlerini ve aboneliklerinizin geçerli Puanını listelemek için **güvenli puan denetimleri** API 'sini de kullanabilirsiniz.

Güvenli Puanlama API 'SI ile mümkün olan dış araçların örnekleri için [GitHub topluluğumuzın güvenli Puanlama alanına](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score)bakın.

[Azure Güvenlik Merkezi 'nde güvenli puan ve güvenlik denetimleri](secure-score-security-controls.md)hakkında daha fazla bilgi edinin.



### <a name="advanced-data-security-for-sql-machines-azure-other-clouds-and-on-prem-preview"></a>SQL makineler için gelişmiş veri güvenliği (Azure, diğer bulutlar ve şirket içi) (Önizleme)

Azure Güvenlik Merkezi 'nin SQL makineler için gelişmiş veri güvenliği artık Azure 'da barındırılan SQL sunucularını diğer bulut ortamlarında ve hatta şirket içi makinelerde korur. Bu, karma ortamları tam olarak desteklemek için Azure Native SQL sunucularınız için korumaların kapsamını genişletir.

Gelişmiş veri güvenliği, SQL makinelerinizde bulundukları yerde güvenlik açığı değerlendirmesi ve Gelişmiş tehdit koruması sağlar.

Ayarla iki adımdan oluşur:

1. Azure hesabı bağlantısı sağlamak için Log Analytics aracısını SQL Server ana makinesine dağıtma.

1. Güvenlik Merkezi 'nin fiyatlandırma ve Ayarlar sayfasında isteğe bağlı paket etkinleştiriliyor.

[SQL makinelerinde gelişmiş veri güvenliği](defender-for-sql-usage.md)hakkında daha fazla bilgi edinin.



### <a name="two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview"></a>Log Analytics aracısını Azure Arc makinelerine dağıtmaya yönelik iki yeni öneri (Önizleme)

[Log Analytics aracısını](../azure-monitor/platform/log-analytics-agent.md) Azure Arc makinelerinize dağıtmaya yardımcı olmak ve Azure Güvenlik Merkezi tarafından korunduğundan emin olmak için iki yeni öneri eklenmiştir:

- **Log Analytics Aracısı Windows tabanlı Azure Arc makinelerinizde yüklü olmalıdır (Önizleme)**
- **Log Analytics Aracısı, Linux tabanlı Azure yay makinelerinizde yüklü olmalıdır (Önizleme)**

Bu yeni öneriler, var olan (ilgili) öneriyle aynı dört güvenlik denetiminde görünür, **makinelerinizde izleme Aracısı yüklü** olmalıdır: güvenlik yapılandırmalarının düzeltilmesi, uyarlamalı uygulama denetimi uygulamak, sistem güncelleştirmelerini uygulamak ve Endpoint Protection 'ı etkinleştirmek.

Öneriler Ayrıca dağıtım sürecini hızlandırmaya yardımcı olmak için hızlı düzelme özelliğini de içerir. 

[İşlem ve uygulama önerileri](recommendations-reference.md#recs-computeapp) tablosunda bu iki yeni öneri hakkında daha fazla bilgi edinin.

Azure Güvenlik Merkezi ['nin aracıyı Log Analytics aracı nedir?](faq-data-collection-agents.md#what-is-the-log-analytics-agent)bölümünde nasıl kullandığı hakkında daha fazla bilgi edinin.

[Azure Arc makinelerinde uzantıları](../azure-arc/servers/manage-vm-extensions.md)hakkında daha fazla bilgi edinin.


### <a name="new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale"></a>Ölçekte sürekli dışa aktarma ve iş akışı Otomasyonu yapılandırması oluşturmaya yönelik yeni ilkeler

Kuruluşunuzun izleme ve olay yanıtı süreçlerini otomatik hale getirmek, güvenlik olaylarını araştırmak ve azaltmak için gereken süreyi büyük ölçüde iyileştirebilir.

Otomasyon yapılandırmalarınızı kuruluşunuz genelinde dağıtmak için bu yerleşik ' DeployIfdNotExist ' Azure ilkelerini kullanarak [sürekli dışa aktarma](continuous-export.md) ve [iş akışı Otomasyonu](workflow-automation.md) yordamları oluşturun ve yapılandırın:

İlkeler Azure ilkesinde bulunabilir:


|Hedef  |İlke  |İlke KIMLIĞI  |
|---------|---------|---------|
|Olay Hub 'ına sürekli dışarı aktarma|[Azure Güvenlik Merkezi uyarıları ve önerileri için Olay Hub'ına dışarı aktarma dağıtımı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fcdfcce10-4578-4ecd-9703-530938e4abcb)|cdfcce10-4578-4ecd-9703-530938e4abcb|
|Log Analytics çalışma alanına sürekli dışarı aktarma|[Azure Güvenlik Merkezi uyarıları ve önerileri için Log Analytics çalışma alanına dışarı aktarma dağıtımı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fffb6f416-7bd2-4488-8828-56585fef2be9)|ffb6f416-7bd2-4488-8828-56585fef2be9|
|Güvenlik uyarıları için iş akışı Otomasyonu|[Azure Güvenlik Merkezi uyarıları için İş Akışı Otomasyonu dağıtımı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2ff1525828-9a90-4fcf-be48-268cdd02361e)|f1525828-9a90-4fcf-be48-268cdd02361e|
|Güvenlik önerileri için iş akışı Otomasyonu|[Azure Güvenlik Merkezi önerileri için İş Akışı Otomasyonu dağıtımı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f73d6ab6c-2475-4850-afd6-43795f3492ef)|73d6ab6c-2475-4850-afd6-43795f3492ef|
||||

[İş akışı Otomasyonu şablonları](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation)ile çalışmaya başlayın.

[Sağlanan ilkeleri kullanarak iş akışı Otomasyonu 'nu ölçekte yapılandırma](workflow-automation.md#configure-workflow-automation-at-scale-using-the-supplied-policies) ve [sürekli dışarı aktarma ayarlama](continuous-export.md#set-up-a-continuous-export)bölümünde iki dışarı aktarma ilkesini kullanma hakkında daha fazla bilgi edinin.


### <a name="new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines"></a>İnternet 'e yönelik olmayan sanal makineleri korumak için NSG 'lerin kullanılmasına yönelik yeni öneri

"Güvenlik en iyi yöntemlerini uygulama" güvenlik denetimi artık aşağıdaki yeni öneriyi içerir:

- **İnternet 'e yönelik olmayan sanal makineler ağ güvenlik gruplarıyla korunmalıdır**

İnternet 'e yönelik sanal makineler, internet 'e yönelik ve internet 'e yönelik olmayan VM 'Ler arasında ayrım yapmadığından, var olan bir önerinin **ağ güvenlik gruplarıyla korunması gerekir**. Her iki için de bir VM bir ağ güvenlik grubuna atanmamışsa yüksek öneme sahip bir öneri oluşturulmuştur. Bu yeni öneri, internet 'e yönelik olmayan makineleri, yanlış pozitif sonuçları azaltmak ve gereksiz yüksek önem derecesine karşı önlemek için ayırır.

[Ağ önerileri](recommendations-reference.md#recs-network) tablosunda daha fazla bilgi edinin.




### <a name="new-policies-for-enabling-threat-protection-and-advanced-data-security"></a>Tehdit koruması ve gelişmiş veri güvenliğini etkinleştirmek için yeni ilkeler

Aşağıdaki yeni ilkeler ASC varsayılan girişimine eklenmiştir ve ilgili kaynak türleri için tehdit koruması veya gelişmiş veri güvenliğini etkinleştirme konusunda yardımcı olmak üzere tasarlanmıştır.

İlkeler Azure ilkesinde bulunabilir:


| İlke                                                                                                                                                                                                                                                                | İlke KIMLIĞI                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| [Azure SQL veritabanı sunucularında gelişmiş veri güvenliği etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f7fe3b40f-802b-4cdd-8bd4-fd799c948cc2)     | 7fe3b40f-802b-4CDD-8bd4-fd799c948cc2 |
| [Makinelerdeki SQL sunucularında gelişmiş veri güvenliği etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6581d072-105e-4418-827f-bd446d56421b) | 6581d072-105E-4418-827f-bd446d56421b |
| [Gelişmiş tehdit koruması, Azure depolama hesaplarında etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f308fbb08-4ab8-4e67-9b29-592e93fb94fa)           | 308fbb08-4ab8-4e67-9b29-592e93fb94fa |
| [Gelişmiş tehdit koruması Azure Key Vault kasaları üzerinde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f0e6763cc-5078-4e64-889d-ff4d9a839047)           | 0e6763cc-5078-4e64-889d-ff4d9a839047 |
| [Gelişmiş tehdit koruması Azure App Service planlarda etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f2913021d-f2fd-4f3d-b958-22354e2bdbcb)                | 2913021d-f2fd-4f3d-b958-22354e2bdbcb |
| [Gelişmiş tehdit koruması Azure Container Registry kayıt defterlerinde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fc25d9a16-bc35-4e15-a7e5-9db606bf9ed4)   | c25d9a16-bc35-4e15-a7e5-9db606bf9ed4 |
| [Gelişmiş tehdit koruması, Azure Kubernetes hizmet kümelerinde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f523b5cd1-3e23-492f-a539-13118b6d1e3a)   | 523b5cd1-3e23-492F-A539-13118b6d1e3a |
| [Gelişmiş tehdit koruması sanal makinelerde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f4da35fc9-c9e7-4960-aec9-797fe7d9051d)           | 4ba35fc9-c9e7-4960-aec9-797fe7d9051d |
|                                                                                                                                                                                                                                                                       |                                      |

[Azure Güvenlik Merkezi 'Nde tehdit koruması](azure-defender.md)hakkında daha fazla bilgi edinin.
