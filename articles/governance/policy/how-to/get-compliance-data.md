---
title: İlke uyumluluğu verilerini alın
description: Azure İlkesi değerlendirmeleri ve efektleri uyumluluğu belirler. Azure kaynaklarınızın uyumluluk ayrıntılarını nasıl alacağınızı öğrenin.
ms.date: 02/01/2019
ms.topic: how-to
ms.openlocfilehash: 891c9c72d8e83dc8f9adb930e8ebd11b70f6aad8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280644"
---
# <a name="get-compliance-data-of-azure-resources"></a>Azure kaynaklarının uyumluluk verilerini alın

Azure İlkesi'nin en büyük avantajlarından biri, abonelik veya [yönetim abonelik grubundaki](../../management-groups/overview.md) kaynaklar üzerinde sağladığı öngörü ve denetimdir. Bu denetim, kaynakların yanlış konumda oluşturulmasını önlemek, yaygın ve tutarlı etiket kullanımını zorlamak veya uygun yapılandırmalar ve ayarlar için varolan kaynakları denetlemek gibi birçok farklı şekilde kullanılabilir. Her durumda, verilerin ortamınızın uyumluluk durumunu anlamanızı sağlamak için Azure İlkesi tarafından oluşturulur.

İlke ve girişim atamalarınız tarafından oluşturulan uyumluluk bilgilerine erişmenin birkaç yolu vardır:

- Azure [portalını](#portal) kullanma
- [Komut satırı](#command-line) komut dosyası aracılığıyla

Uyumluluk hakkında rapor verme yöntemlerine bakmadan önce, uyumluluk bilgilerinin ne zaman güncelleştirileceklerine ve bir değerlendirme döngüsünü tetikleyen sıklık ve olaylara bakalım.

> [!WARNING]
> Uyumluluk durumu **kayıtlı değilse,** **Microsoft.PolicyInsights** Kaynak Sağlayıcısı'nın kayıtlı olduğunu ve kullanıcının [Azure Politikası'nda RBAC'da](../overview.md#rbac-permissions-in-azure-policy)açıklandığı gibi uygun rol tabanlı erişim denetimine (RBAC) izinlere sahip olduğunu doğrulayın.

## <a name="evaluation-triggers"></a>Değerlendirme tetikleyicileri

Tamamlanmış bir değerlendirme döngüsünün sonuçları `Microsoft.PolicyInsights` Kaynak Sağlayıcı'da `PolicyStates` `PolicyEvents` kullanılabilir. Azure İlkesi Öngörüleri REST API'nin işlemleri hakkında daha fazla bilgi için [Azure İlkesi Öngörüleri'ne](/rest/api/policy-insights/)bakın.

Atanan politika ve girişimlerin değerlendirmeleri çeşitli olayların sonucu olarak gerçekleşir:

- Bir ilke veya girişim bir kapsama yeni atanır. Atamanın tanımlanan kapsama uygulanması yaklaşık 30 dakika sürer. Uygulandıktan sonra, bu kapsamdaki kaynaklar için değerlendirme döngüsü yeni atanan ilke veya girişime karşı başlar ve ilke veya girişim tarafından kullanılan etkilere bağlı olarak, kaynaklar uyumlu veya uyumlu olmayan olarak işaretlenir. Kaynakların büyük bir kapsamına karşı değerlendirilen büyük bir politika veya girişim zaman alabilir. Bu nedenle, değerlendirme döngüsünün ne zaman tamamlanacağına dair önceden tanımlanmış bir beklenti yoktur. Tamamlandığında, güncelleştirilmiş uyumluluk sonuçları portal ve SDK'larda kullanılabilir.

- Bir kapsama zaten atanmış bir ilke veya girişim güncelleştirilir. Bu senaryonun değerlendirme döngüsü ve zamanlaması, bir kapsama yeni bir atama yla aynıdır.

- Kaynak Yöneticisi, REST, Azure CLI veya Azure PowerShell aracılığıyla bir atama yla bir kapsama dağıtılır. Bu senaryoda, tek tek kaynak için efekt olayı (ek, denetim, reddetme, dağıtma) ve uyumlu durum bilgileri portalda ve SDK'larda yaklaşık 15 dakika sonra kullanılabilir hale gelir. Bu olay diğer kaynakların değerlendirilmesini neden olmaz.

- Standart uyumluluk değerlendirme döngüsü. Her 24 saatte bir, atamalar otomatik olarak yeniden değerlendirilir. Birçok kaynağın büyük bir politikası veya girişimi zaman alabilir, bu nedenle değerlendirme döngüsünün ne zaman tamamlanacağına dair önceden tanımlanmış bir beklenti yoktur. Tamamlandığında, güncelleştirilmiş uyumluluk sonuçları portal ve SDK'larda kullanılabilir.

- [Konuk Yapılandırma](../concepts/guest-configuration.md) kaynak sağlayıcısı, yönetilen bir kaynak tarafından uyumluluk ayrıntılarıyla güncelleştirilir.

- İsteğe bağlı teşp

### <a name="on-demand-evaluation-scan"></a>İsteğe bağlı değerlendirme taraması

Rest API'ye yapılan bir çağrıyla abonelik veya kaynak grubu için değerlendirme taramayı başlatılabilir. Bu talan asynchronous bir işlemdir. Bu nedenle, tmayı başlatmak için REST bitiş noktası, tamanın yanıt vermesi için tamamlanmayı beklemez. Bunun yerine, istenen değerlendirme durumunu sorgulamak için bir URI sağlar.

Her bir REST API URI'sinde kendi değerlerinizle değiştirmeniz gereken değişkenler bulunur:

- `{YourRG}`- Kaynak grubunuzun adı ile değiştirin
- `{subscriptionId}` - Abonelik kimliğinizle değiştirin

Scan, abonelikteki veya kaynak grubundaki kaynakların değerlendirilmesini destekler. Aşağıdaki URI yapılarını kullanarak REST API **POST** komutu yla kapsam bazında bir tarama başlatın:

- Abonelik

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

- Kaynak grubu

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{YourRG}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

Arama **202 Kabul edilen** durumu döndürür. Yanıt üstbilgisinde aşağıdaki biçime sahip bir **Konum** özelliği de yer almaktadır:

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/asyncOperationResults/{ResourceContainerGUID}?api-version=2018-07-01-preview
```

`{ResourceContainerGUID}`istenen kapsam için statik olarak oluşturulur. Kapsam zaten isteğe bağlı bir tarak çalıştırıyorsa, yeni bir teşleme başlatılma.. Bunun yerine, yeni istek `{ResourceContainerGUID}` durum için aynı **konum** URI sağlanır. **Konum** URI bir REST API **GET** komutu bir **202 Kabul değerlendirme** devam ederken döndürür. Değerlendirme stomu tamamlandığında, **200 Ok** durumu döndürür. Tamamlanmış bir tama gövdesi durumu ile bir JSON yanıtı:

```json
{
    "status": "Succeeded"
}
```

## <a name="how-compliance-works"></a>Uyumluluk nasıl çalışır?

Bir atamada, bir kaynak ilke veya girişim kurallarına uymuyorsa **uyumlu** değildir.
Aşağıdaki tablo, farklı ilke efektlerinin, ortaya çıkan uyumluluk durumu için durum değerlendirmesiyle nasıl çalıştığını gösterir:

| Kaynak durumu | Etki | İlke değerlendirmesi | Uyumluluk durumu |
| --- | --- | --- | --- |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Uyumlu değil |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | True | Uyumlu değil |
| Yeni | Audit, AuditIfNotExist\* | False | Uyumlu |

\* Append, DeployIfNotExist ve AuditIfNotExist etkileri IF deyiminin TRUE olmasını gerektirir.
Etkiler ayrıca varlık koşulunun uyumlu olmaması için FALSE olmasını gerektirir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

Örneğin, ortak ağlara açık bazı depolama hesapları (kırmızı ile vurgulanmış) ile bir kaynak grubunuz olduğunu varsayalım.

![Ortak ağlara açık depolama hesapları](../media/getting-compliance-data/resource-group01.png)

Bu örnekte, güvenlik risklerine karşı dikkatli olmanız gerekir. Artık bir ilke ataması oluşturduğunuza göre, ContosoRG kaynak grubundaki tüm depolama hesapları için değerlendirilir. Uyumsuz üç depolama hesabını denetler ve bu nedenle durumlarını uyumsuz olarak **değiştirir.**

![Denetlenen uyumlu olmayan depolama hesapları](../media/getting-compliance-data/resource-group03.png)

**Uyumlu** ve **Uyumlu Olmayan,** politikaları ve kaynakları nın yanı sıra üç durum daha vardır:

- **Çakışan**: İki veya daha fazla ilke çakışan kurallarla vardır. Örneğin, aynı etiketi farklı değerlerle ekalan iki ilke.
- **Başlatılmadı**: İlke veya kaynak için değerlendirme döngüsü başlamadı.
- **Kayıtlı Değil**: Azure İlke Kaynak Sağlayıcısı kaydedilmedi veya oturum açan hesabın uyumluluk verilerini okuma izni yok.

Azure İlkesi, kaynağın eşleşip eşleşmeyecek olmadığını belirlemek için tanımdaki **tür** ve **ad** alanlarını kullanır. Kaynak eşleştiğinde, uygulanabilir kabul edilir ve **Uyumlu** veya **Uyumlu olmayan**bir duruma sahiptir. **Tanımdaki** tek özellik tür veya **ad** ise, tüm kaynaklar geçerli kabul edilir ve değerlendirilir.

Uyumluluk yüzdesi, **Uyumlu** kaynakların toplam _kaynaklara_bölünmesiyle belirlenir.
_Toplam_ **kaynaklar, Uyumlu, Uyumlu** **Olmayan**ve **Çakışan** kaynakların toplamı olarak tanımlanır. Genel uyumluluk sayıları, **Uyumlu** tüm farklı kaynakların toplamına bölünen farklı kaynakların toplamıdır. Aşağıdaki resimde, geçerli olan 20 farklı kaynak vardır ve yalnızca biri **uyumlu değildir.** Toplam kaynak uyumluluğu %95'tir (20 üzerinden 19).

![Uyumluluk sayfasından ilke uyumluluğu örneği](../media/getting-compliance-data/simple-compliance.png)

## <a name="portal"></a>Portal

Azure portalı, ortamınızdaki uyumluluk durumunu görselleştirme ve anlama da grafiksel bir deneyim sunar. **İlke** sayfasında, **Genel Bakış** seçeneği, hem politikaların hem de girişimlerin uygunluğu na ilişkin kullanılabilir kapsamlar için ayrıntılar sağlar. Uyumluluk durumu ve atama başına sayımla birlikte, son yedi gün içinde uyumluluğu gösteren bir grafik içerir. **Uyumluluk** sayfası bu aynı bilgilerin çoğunu (grafik hariç) içerir, ancak ek filtreleme ve sıralama seçenekleri sağlar.

![Azure İlke Uyumluluğu sayfası örneği](../media/getting-compliance-data/compliance-page.png)

Bir ilke veya girişim farklı kapsamlara atanabildiği için, tablo her atamanın kapsamını ve atanan tanım türünü içerir. Her atama için uyumsuz kaynakların ve uyumsuz ilkelerin sayısı da sağlanır. Tablodaki bir ilke veya girişimi tıklatmak, belirli bir atamaya uygunluk hakkında daha derin bir bakış sağlar.

![Azure İlke Uyumluluk Ayrıntıları sayfası örneği](../media/getting-compliance-data/compliance-details.png)

**Kaynak uyumluluğu** sekmesindeki kaynakların listesi, geçerli atama için varolan kaynakların değerlendirme durumunu gösterir. Sekme varsayılan olarak **uyumlu değil,** ancak filtrelenebilir.
Kaynak oluşturma isteğitarafından tetiklenen olaylar (ek, denetim, reddetme, dağıtma) **Olaylar** sekmesialtında gösterilir.

> [!NOTE]
> AKS Engine ilkesi için gösterilen kaynak, kaynak grubudur.

![Azure İlke Uyumluluğu etkinlikleri örneği](../media/getting-compliance-data/compliance-events.png)

[Kaynak Sağlayıcı modu kaynakları](../concepts/definition-structure.md#resource-provider-modes) için Kaynak **uyumluluk** sekmesinde, kaynağı seçmek veya satıra sağ tıklayarak ve Uyumluluk **ayrıntılarını görüntüle'yi** seçmek bileşen uyumluluk ayrıntılarını açar. Bu sayfa ayrıca bu kaynağa atanan ilkeleri, olayları, bileşen olaylarını ve geçmişdeğiştirmeyi görmek için sekmeler de sunar.

![Azure İlkeri Bileşeni uyumluluk ayrıntıları örneği](../media/getting-compliance-data/compliance-components.png)

Kaynak uyumluluk sayfasında, daha fazla ayrıntı toplamak istediğiniz etkinliğin satırına sağ tıklayın ve **etkinlik günlüklerini göster'i**seçin. Etkinlik günlüğü sayfası açılır ve atama ve olaylariçin ayrıntıları gösteren aramaya önceden filtrelenir. Etkinlik günlüğü ek bağlam ve bu olaylar hakkında bilgi sağlar.

![Azure İlke Uyumluluğu Etkinlik Günlüğü Örneği](../media/getting-compliance-data/compliance-activitylog.png)

### <a name="understand-non-compliance"></a>Uyumsuzluğu anlama

<a name="change-history-preview"></a>

Bir kaynağın uyumlu **olmadığı**belirlendiğinde, birçok olası nedeni vardır. Bir kaynağın neden **uyumlu olmadığını** belirlemek veya değişikliği sorumlu bulmak için [bkz.](./determine-non-compliance.md)

## <a name="command-line"></a>Komut satırı

Portalda bulunan aynı bilgiler REST API [(ARMClient](https://github.com/projectkudu/ARMClient)dahil), Azure PowerShell ve Azure CLI (önizleme) ile alınabilir.
REST API ile ilgili tüm ayrıntılar için [Azure İlkesi Öngörüleri](/rest/api/policy-insights/) başvurusuna bakın. REST API başvuru sayfalarında her işlemde tarayıcıda doğru şekilde denemenizi sağlayan yeşil bir 'Try It' düğmesi bulunmaktadır.

REST API örnekleri için Azure'da kimlik doğrulamayı işlemek için ARMClient veya benzer bir aracı kullanın.

### <a name="summarize-results"></a>Sonuçları özetle

REST API ile özetleme kapsayıcı, tanım veya atama ile gerçekleştirilebilir. Azure İlkesi Insight'ın [Abonelik Için Özetle'sini](/rest/api/policy-insights/policystates/summarizeforsubscription)kullanarak abonelik düzeyinde özetleme örneği aşağıda verilmiştir:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04
```

Çıktı, aboneliği özetler. Aşağıdaki örnek çıktıda, özetlenen uyumluluk **value.results.nonCompliantResources** ve **value.results.nonCompliantPolicies**altındadır. Bu istek, uyumlu olmayan numaraları oluşturan her atama ve her atama için tanım bilgileri dahil olmak üzere daha fazla ayrıntı sağlar. Hiyerarşideki her ilke nesnesi, bu düzeyde ek ayrıntı almak için kullanılabilecek bir **queryResultsUri** sağlar.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary/$entity",
        "results": {
            "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false",
            "nonCompliantResources": 15,
            "nonCompliantPolicies": 1
        },
        "policyAssignments": [{
            "policyAssignmentId": "/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77",
            "policySetDefinitionId": "",
            "results": {
                "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77'",
                "nonCompliantResources": 15,
                "nonCompliantPolicies": 1
            },
            "policyDefinitions": [{
                "policyDefinitionReferenceId": "",
                "policyDefinitionId": "/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "effect": "deny",
                "results": {
                    "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'",
                    "nonCompliantResources": 15
                }
            }]
        }]
    }]
}
```

### <a name="query-for-resources"></a>Kaynaklar için sorgu

Yukarıdaki örnekte, **value.policyAssignments.policyDefinitions.results.queryResultsUri** belirli bir ilke tanımı için tüm uyumlu olmayan kaynaklar için örnek Uri sağlar. **$filter** değerine bakıldığında, IsCompliant false'a (eq) eşittir, PolicyAssignmentId ilke tanımı için belirtilir ve ardından PolicyDefinitionId'in kendisi belirtilir. PolicyAssignmentId'in filtreye eklenmesinin nedeni, PolicyDefinitionId'in farklı kapsamlara sahip çeşitli ilke veya girişim atamalarında bulunabiliyor olmasıdır. Hem PolicyAssignmentId hem de PolicyDefinitionId'i belirterek, aradığımız sonuçlarda açık olabiliriz. Daha önce, Poliçe Ler için, son 24 saatin in ve **saat** penceresini otomatik olarak ayarlayan **en son**, kullandık. **from**

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Aşağıdaki örnek yanıt, kısaltma için tek bir uyumsuz kaynağa kesilmiştir. Ayrıntılı yanıt, kaynak, ilke veya girişim ve atama hakkında çeşitli veri parçaları vardır. İlke tanımına hangi atama parametrelerinin geçirildiğini de görebildiğinize dikkat edin.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 15,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "timestamp": "2018-05-19T04:41:09Z",
        "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Compute/virtualMachines/linux",
        "policyAssignmentId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Authorization/policyAssignments/37ce239ae4304622914f0c77",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "effectiveParameters": "",
        "isCompliant": false,
        "subscriptionId": "{subscriptionId}",
        "resourceType": "/Microsoft.Compute/virtualMachines",
        "resourceLocation": "westus2",
        "resourceGroup": "RG-Tags",
        "resourceTags": "tbd",
        "policyAssignmentName": "37ce239ae4304622914f0c77",
        "policyAssignmentOwner": "tbd",
        "policyAssignmentParameters": "{\"tagName\":{\"value\":\"costCenter\"},\"tagValue\":{\"value\":\"Contoso-Test\"}}",
        "policyAssignmentScope": "/subscriptions/{subscriptionId}/resourceGroups/RG-Tags",
        "policyDefinitionName": "1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "policyDefinitionAction": "deny",
        "policyDefinitionCategory": "tbd",
        "policySetDefinitionId": "",
        "policySetDefinitionName": "",
        "policySetDefinitionOwner": "",
        "policySetDefinitionCategory": "",
        "policySetDefinitionParameters": "",
        "managementGroupIds": "",
        "policyDefinitionReferenceId": ""
    }]
}
```

### <a name="view-events"></a>Etkinlikleri görüntüleme

Bir kaynak oluşturulduğunda veya güncelleştirildiğinde, ilke değerlendirme sonucu oluşturulur. Sonuçlara _ilke olayları_denir. Abonelikle ilişkili son ilke olaylarını görüntülemek için aşağıdaki Uri'yi kullanın.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2018-04-04
```

Sonuçlarınız aşağıdaki örneğe benzer:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
        "NumAuditEvents": 16
    }]
}
```

İlke olaylarını sorgulama hakkında daha fazla bilgi için [Azure İlkesi Etkinlikleri](/rest/api/policy-insights/policyevents) başvuru makalesine bakın.

### <a name="azure-powershell"></a>Azure PowerShell

Azure İlkesi için Azure PowerShell modülü PowerShell Galerisi'nde [Az.PolicyInsights](https://www.powershellgallery.com/packages/Az.PolicyInsights)olarak kullanılabilir.
PowerShellGet'i kullanarak `Install-Module -Name Az.PolicyInsights` modülü yükleyebilirsiniz (en son [Azure PowerShell](/powershell/azure/install-az-ps) yüklü olduğundan emin olun):

```azurepowershell-interactive
# Install from PowerShell Gallery via PowerShellGet
Install-Module -Name Az.PolicyInsights

# Import the downloaded module
Import-Module Az.PolicyInsights

# Login with Connect-AzAccount if not using Cloud Shell
Connect-AzAccount
```

Modülde aşağıdaki cmdlets vardır:

- `Get-AzPolicyStateSummary`
- `Get-AzPolicyState`
- `Get-AzPolicyEvent`
- `Get-AzPolicyRemediation`
- `Remove-AzPolicyRemediation`
- `Start-AzPolicyRemediation`
- `Stop-AzPolicyRemediation`

Örnek: En yüksek düzeyde uyumsuz kaynak sayısına sahip en üst atanan ilke için durum özetini alma.

```azurepowershell-interactive
PS> Get-AzPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Örnek: En son değerlendirilen kaynak için durum kaydının alınır (varsayılan değer azalan sırada zaman damgasına göredir).

```azurepowershell-interactive
PS> Get-AzPolicyState -Top 1

Timestamp                  : 5/22/2018 3:47:34 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/networkInterfaces/linux316
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/networkInterfaces
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Örnek: Uyumlu olmayan tüm sanal ağ kaynaklarının ayrıntılarını alma.

```azurepowershell-interactive
PS> Get-AzPolicyState -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'"

Timestamp                  : 5/22/2018 4:02:20 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Örnek: Belirli bir tarihten sonra gerçekleşen uyumlu olmayan sanal ağ kaynaklarıyla ilgili olayları alma.

```azurepowershell-interactive
PS> Get-AzPolicyEvent -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'" -From '2018-05-19'

Timestamp                  : 5/19/2018 5:18:53 AM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : eastus
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
TenantId                   : {tenantId}
PrincipalOid               : {principalOid}
```

**PrincipalOid** alanı, Azure PowerShell cmdlet'li `Get-AzADUser`belirli bir kullanıcıyı elde etmek için kullanılabilir. **{principalOid}** adresini önceki örnekten aldığınız yanıtla değiştirin.

```azurepowershell-interactive
PS> (Get-AzADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Aboneliğinize bağlı [Activity Log Analytics](../../../azure-monitor/platform/activity-log-collect.md) `AzureActivity` `AzureActivity` çözümünden bir Log [Analytics çalışma alanınız](../../../log-analytics/log-analytics-overview.md) varsa, basit Kusto sorgularını ve tabloyu kullanarak değerlendirme döngüsündeki uyumsuzluk sonuçlarını da görüntüleyebilirsiniz. Azure Monitor günlüklerinde ayrıntılarla, uyarılar uyumsuzluğu izleyecek şekilde yapılandırılabilir.


![Azure Monitör günlüklerini kullanarak Azure İlke Uyumluluğu](../media/getting-compliance-data/compliance-loganalytics.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](programmatically-create.md)anlayın.
- [Uyumlu olmayan kaynakları](remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.