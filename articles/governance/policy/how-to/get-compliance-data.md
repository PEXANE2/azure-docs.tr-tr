---
title: İlke uyumluluk verilerini al
description: Azure Ilke değerlendirmeleri ve etkileri uyumluluğu tespit edin. Azure kaynaklarınızın uyumluluk ayrıntılarını nasıl alabileceğinizi öğrenin.
ms.date: 05/20/2020
ms.topic: how-to
ms.openlocfilehash: 55f0b471eff15140de0a586fd5d326d9cd913b1a
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83747080"
---
# <a name="get-compliance-data-of-azure-resources"></a>Azure kaynaklarının uyumluluk verilerini alın

Azure Ilkesinin en büyük avantajlarından biri, bir abonelik ya da bir abonelik [Yönetim grubundaki](../../management-groups/overview.md) kaynaklar üzerinde sağladığı Öngörüler ve denetimlerdir. Bu denetim, kaynakları yanlış konumda oluşturulmasını önlemek, ortak ve tutarlı etiket kullanımı uygulanmasını sağlamak ya da uygun yapılandırma ve ayarlar için mevcut kaynakları denetlemek gibi birçok farklı yolla uygulanabilir. Her durumda, ortamınız uyumluluk durumunu anlamanıza olanak tanımak için Azure Ilkesi tarafından veriler oluşturulur.

İlke ve girişim atamalarınız tarafından oluşturulan uyumluluk bilgilerine erişmenin birkaç yolu vardır:

- [Azure Portal](#portal) kullanma
- [Komut satırı](#command-line) komut dosyası aracılığıyla

Uyumluluk hakkında raporlama yöntemlerine bakmadan önce uyumluluk bilgilerinin ne zaman güncelleştirildiğini ve bir değerlendirme döngüsünü tetikleyen sıklığı ve olayları inceleyelim.

> [!WARNING]
> Uyumluluk durumu **kayıtlı değil**olarak bildiriliyorsa, **Microsoft. Policınsıghts** kaynak sağlayıcısının kayıtlı olduğunu ve kullanıcının [Azure ilkesinde RBAC](../overview.md#rbac-permissions-in-azure-policy)'de açıklandığı şekilde uygun rol tabanlı erişim denetimi (RBAC) izinlerine sahip olduğunu doğrulayın.

## <a name="evaluation-triggers"></a>Değerlendirme Tetikleyicileri

Tamamlanmış bir değerlendirme döngüsünün sonuçları, `Microsoft.PolicyInsights` kaynak sağlayıcıda `PolicyStates` ve işlemler aracılığıyla kullanılabilir `PolicyEvents` . Azure Policy Insights REST API işlemleri hakkında daha fazla bilgi için bkz. [Azure Ilke öngörüleri](/rest/api/policy-insights/).

Atanan ilkelerin ve girişimlerin değerlendirmeleri çeşitli olayların sonucu olarak gerçekleşir:

- Bir ilke veya girişim yeni bir kapsama atanır. Atamanın tanımlanan kapsama uygulanması 30 dakika içinde sürer. Bu bir kez uygulandıktan sonra, değerlendirme çevrimi, bu kapsamdaki kaynaklar için yeni atanan ilke veya girişim ile başlar ve ilke ya da girişim tarafından kullanılan etkilere bağlı olarak, kaynaklar uyumlu veya uyumlu değil olarak işaretlenir. Büyük bir kaynak kapsamı için değerlendirilen büyük bir ilke veya girişim zaman alabilir. Bu nedenle, değerlendirme döngüsünün tamamlandığı zaman önceden tanımlı bir beklentisi yoktur. İşlem tamamlandıktan sonra portalda ve SDK 'larda güncelleştirilmiş uyumluluk sonuçları bulunur.

- Bir kapsama zaten atanmış olan bir ilke veya girişim güncellenir. Bu senaryonun değerlendirme çevrimi ve zamanlaması, bir kapsama yönelik yeni atama ile aynıdır.

- Bir kaynak, Kaynak Yöneticisi, REST, Azure CLı veya Azure PowerShell aracılığıyla atama içeren bir kapsama dağıtılır. Bu senaryoda, tek tek kaynak için etkinlik olayı (ekleme, denetim, reddetme, dağıtma) ve uyumlu durum bilgileri portalda ve bu süre içinde 15 dakika daha sonra SDK 'larda kullanılabilir hale gelir. Bu olay diğer kaynakların değerlendirilmesine neden olmaz.

- Standart uyumluluk değerlendirme çevrimi. Her 24 saatte bir, atamalar otomatik olarak yeniden değerlendirilecektir. Çok sayıda kaynak için büyük bir ilke veya girişim zaman alabilir, bu nedenle değerlendirme döngüsünün tamamlandığı zaman önceden tanımlanmış bir beklentisi yoktur. İşlem tamamlandıktan sonra portalda ve SDK 'larda güncelleştirilmiş uyumluluk sonuçları bulunur.

- [Konuk yapılandırma](../concepts/guest-configuration.md) kaynak sağlayıcısı, yönetilen bir kaynak tarafından uyumluluk ayrıntılarıyla güncelleştirildi.

- İsteğe bağlı tarama

### <a name="on-demand-evaluation-scan"></a>İsteğe bağlı değerlendirme taraması

Bir abonelik veya kaynak grubu için bir değerlendirme taraması, Azure PowerShell veya REST API çağrısıyla başlatılabilir. Bu tarama zaman uyumsuz bir işlemdir.

#### <a name="on-demand-evaluation-scan---azure-powershell"></a>İsteğe bağlı değerlendirme taraması-Azure PowerShell

Uyumluluk taraması [Start-Azpolicykarmaşıkancescan](/powershell/module/az.policyinsights/start-azpolicycompliancescan) cmdlet 'i ile başlatılır.

Varsayılan olarak, `Start-AzPolicyComplianceScan` geçerli abonelikteki tüm kaynaklar için bir değerlendirme başlatır. Belirli bir kaynak grubunda bir değerlendirmeyi başlatmak için **Resourcegroupname** parametresini kullanın. Aşağıdaki örnek, _Myrg_ kaynak grubu için geçerli abonelikte bir uyumluluk taraması başlatır:

```azurepowershell-interactive
Start-AzPolicyComplianceScan -ResourceGroupName MyRG
```

Zaman uyumsuz çağrının sonuçları elde etmeden önce veya bir [iş](/powershell/module/microsoft.powershell.core/about/about_jobs)arka planda çalışmasını sağlamak için PowerShell 'in tamamlanmasını beklemesini sağlayabilirsiniz. Uyumluluk taramasını arka planda çalıştırmak üzere bir PowerShell işi kullanmak için, **AsJob** parametresini kullanın ve değeri bu örnekte olduğu gibi bir nesne olarak ayarlayın `$job` :

```azurepowershell-interactive
$job = Start-AzPolicyComplianceScan -AsJob
```

Nesneyi denetleyerek, işin durumunu kontrol edebilirsiniz `$job` . İş türündedir `Microsoft.Azure.Commands.Common.AzureLongRunningJob` . `Get-Member` `$job` Kullanılabilir özellikleri ve yöntemleri görmek için nesnesi üzerinde kullanın.

Uyumluluk taraması çalışırken, `$job` nesne aşağıdaki gibi sonuçlar verir:

```azurepowershell-interactive
$job

Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
2      Long Running O… AzureLongRunni… Running       True            localhost            Start-AzPolicyCompliance…
```

Uyumluluk taraması tamamlandığında **durum** özelliği _tamamlandı_olarak değişir.

#### <a name="on-demand-evaluation-scan---rest"></a>İsteğe bağlı değerlendirme taraması-REST

Zaman uyumsuz bir işlem olarak, taramayı başlatmak için REST uç noktası, taramanın yanıt vermesi tamamlanana kadar beklemez. Bunun yerine, istenen değerlendirmenin durumunu sorgulamak için bir URI sağlar.

Her bir REST API URI'sinde kendi değerlerinizle değiştirmeniz gereken değişkenler bulunur:

- `{YourRG}`-Kaynak grubunuzun adıyla değiştirin
- `{subscriptionId}` - Abonelik kimliğinizle değiştirin

Tarama, bir abonelikteki veya bir kaynak grubundaki kaynakların değerlendirilmesini destekler. Aşağıdaki URI yapılarını kullanarak bir REST API **Post** komutuyla kapsama göre tarama başlatın:

- Abonelik

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2019-10-01
  ```

- Kaynak grubu

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{YourRG}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2019-10-01
  ```

Çağrı, **202 kabul edilen** bir durum döndürür. Yanıt üst bilgisine eklenen bir **konum** özelliği aşağıdaki biçimdedir:

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/asyncOperationResults/{ResourceContainerGUID}?api-version=2019-10-01
```

`{ResourceContainerGUID}`, istenen kapsam için statik olarak oluşturulur. Bir kapsam zaten bir isteğe bağlı tarama çalıştırıyorsa yeni bir tarama başlatılmaz. Bunun yerine, yeni istek `{ResourceContainerGUID}` durum için aynı **konum** URI 'si olarak sağlanır. **Konum** URI 'sine yönelik bir REST API **Get** komutu, değerlendirme devam ederken **kabul edilen bir 202** döndürür. Değerlendirme taraması tamamlandığında, **200 bir Tamam** durumu döndürür. Tamamlanmış taramanın gövdesi, şu duruma sahip bir JSON yanıtdır:

```json
{
    "status": "Succeeded"
}
```

## <a name="how-compliance-works"></a>Uyumluluk nasıl işe yarar?

Bir atamada, ilke veya girişim kurallarını takip etmez bir kaynak **uyumlu** değildir.
Aşağıdaki tabloda, farklı ilke efektlerinin, sonuçta elde edilen uyumluluk durumu için koşul değerlendirmesiyle nasıl çalıştığı gösterilmektedir:

| Kaynak durumu | Etki | İlke değerlendirmesi | Uyumluluk durumu |
| --- | --- | --- | --- |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Uyumlu değil |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | True | Uyumlu değil |
| Yeni | Audit, AuditIfNotExist\* | False | Uyumlu |

\* Append, DeployIfNotExist ve AuditIfNotExist etkileri IF deyiminin TRUE olmasını gerektirir.
Etkiler ayrıca varlık koşulunun uyumlu olmaması için FALSE olmasını gerektirir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

Örneğin, ortak ağlara sunulan bazı depolama hesaplarıyla (kırmızı renkle vurgulanmış) bir kaynak grubunuz olduğunu varsayalım.

:::image type="content" source="../media/getting-compliance-data/resource-group01.png" alt-text="Ortak ağlara açık olan depolama hesapları" border="false":::

Bu örnekte, güvenlik riskleri konusunda dikkatli olmanız gerekir. Artık bir ilke ataması oluşturduğunuza göre, ContosoRG kaynak grubundaki tüm depolama hesapları için değerlendirilir. Bu, uyumlu olmayan üç depolama hesabını denetler, sonuç olarak durumlarını **uyumlu değil** olarak değiştirir.

:::image type="content" source="../media/getting-compliance-data/resource-group03.png" alt-text="Denetlenen, uyumlu olmayan depolama hesapları" border="false":::

**Uyumlu** ve **uyumlu olmayan**, ilkelerin ve kaynakların yanında üç farklı durum vardır:

- **Çakışıyor**: çakışan kurallara sahip iki veya daha fazla ilke var. Örneğin, iki ilke aynı etiketi farklı değerlerle yeniden ekleniyor.
- **Başlamadı**: ilke veya kaynak için değerlendirme çevrimi başlatılmadı.
- **Kayıtlı değil**: Azure Ilke kaynak sağlayıcısı kayıtlı değil veya oturum açmış hesabın uyumluluk verilerini okuma izni yok.

Azure Ilkesi, bir kaynağın eşleşme olup olmadığını anlamak için tanımdaki **tür** ve **ad** alanlarını kullanır. Kaynak eşleştiğinde, geçerli kabul edilir ve durumu **uyumlu** veya **uyumsuz**olarak değerlendirilir. Her iki **tür** veya **ad** tanımdaki tek özelliktir, tüm kaynaklar uygulanabilir kabul edilir ve değerlendirilir.

Uyumluluk yüzdesi, **uyumlu** kaynakları _Toplam kaynağa_bölerek belirlenir.
_Toplam kaynak_ , **uyumlu**, **uyumlu olmayan**ve **Çakışan** kaynakların toplamı olarak tanımlanır. Genel uyumluluk numaraları, tüm ayrı kaynakların **toplamına ayrılan ayrı** kaynakların toplamıdır. Aşağıdaki görüntüde, uygulanabilir ve yalnızca bir tane **uyumlu olmayan**20 farklı kaynak vardır. Genel kaynak uyumluluğu %95 ' dir (19/20).

:::image type="content" source="../media/getting-compliance-data/simple-compliance.png" alt-text="Uyumluluk sayfasından ilke uyumluluğu örneği" border="false":::

## <a name="portal"></a>Portal

Azure portal, ortamınızdaki uyumluluk durumunu görselleştirmenin ve anlayabilecek grafiksel bir deneyim gösterir. **İlke** sayfasında **genel bakış** seçeneği, hem ilkelerin hem de girişimlerin uyumluluğuyla kullanılabilir kapsamlar için ayrıntılar sağlar. Atama başına uyumluluk durumu ve sayı ile birlikte, son yedi güne ait uyumluluğu gösteren bir grafik içerir. **Uyumluluk** sayfası, bu bilgilerin büyük bölümünü (grafik dışında) içerir, ancak ek filtreleme ve sıralama seçenekleri sağlar.

:::image type="content" source="../media/getting-compliance-data/compliance-page.png" alt-text="Azure Ilkesi uyumluluk sayfası örneği" border="false":::

Bir ilke veya girişim farklı kapsamlara atanabileceği için tablo, her atamanın kapsamını ve atanan tanım türünü içerir. Her atamaya yönelik uyumlu olmayan kaynakların ve uyumlu olmayan ilkelerin sayısı da sağlanır. Tablodaki bir ilkeye veya girişimye tıkladığınızda, söz konusu atamaya yönelik uyumluluğa daha ayrıntılı bir bakış sunulmaktadır.

:::image type="content" source="../media/getting-compliance-data/compliance-details.png" alt-text="Azure Ilke uyumluluğu ayrıntıları sayfası örneği" border="false":::

**Kaynak uyumluluğu** sekmesindeki kaynakların listesi, geçerli atama için mevcut kaynakların değerlendirme durumunu gösterir. Sekme varsayılan olarak **uyumlu değildir**, ancak filtrelenebilir.
Kaynak oluşturma isteği tarafından tetiklenen olaylar (Append, denetle, Reddet, dağıt), **Olaylar** sekmesi altında gösterilir.

> [!NOTE]
> AKS altyapı ilkesi için, gösterilen kaynak kaynak grubudur.

:::image type="content" source="../media/getting-compliance-data/compliance-events.png" alt-text="Azure Ilkesi uyumluluk olayları örneği" border="false":::

Kaynak [sağlayıcısı modu](../concepts/definition-structure.md#resource-provider-modes) kaynakları için kaynak **uyumluluğu** sekmesinde, kaynağı seçip satırı sağ tıklatıp **görünüm uyumluluk ayrıntıları** ' nı seçerek bileşen uyumluluk ayrıntılarını açılır. Bu sayfa, bu kaynağa, olaylara, bileşen olaylarına ve değişiklik geçmişine atanan ilkeleri görmek için de sekmeler sağlar.

:::image type="content" source="../media/getting-compliance-data/compliance-components.png" alt-text="Azure Ilke bileşeni uyumluluk ayrıntıları örneği" border="false":::

Kaynak uyumluluğu sayfasına dönün, daha fazla ayrıntı toplamak istediğiniz olayın satırına sağ tıklayın ve **etkinlik günlüklerini göster**' i seçin. Etkinlik günlüğü sayfası açılır ve atamanın ayrıntılarını ve olayları gösteren aramaya önceden filtrelenmiştir. Etkinlik günlüğü ek bağlam ve bu olaylar hakkında bilgi sağlar.

:::image type="content" source="../media/getting-compliance-data/compliance-activitylog.png" alt-text="Azure Ilke uyumluluğu etkinlik günlüğü örneği" border="false":::

### <a name="understand-non-compliance"></a>Uyumsuzluğun anlaşılma

Bir kaynağın **uyumsuz**olduğu belirlendiğinde birçok olası neden vardır. Kaynağın **uyumsuz** olma nedenini öğrenmek veya sorumluyu bulmak için bkz. [uyumsuzluğu belirleme](./determine-non-compliance.md).

## <a name="command-line"></a>Komut satırı

Portalda bulunan bilgiler REST API ( [Armclient](https://github.com/projectkudu/ARMClient)dahil), Azure PowerShell ve Azure CLI (Önizleme) ile birlikte alınabilir.
REST API hakkında tam Ayrıntılar için bkz. [Azure Policy Insights](/rest/api/policy-insights/) başvurusu. REST API başvuru sayfalarında, her bir işlemin üzerinde yeşil bir ' deneyin ' düğmesi vardır ve bu, tarayıcıyı tarayıcıda doğrudan denemenize olanak tanır.

REST API örnekleri için Azure kimlik doğrulamasını işlemek üzere ARMClient veya benzer bir araç kullanın.

### <a name="summarize-results"></a>Sonuçları özetleme

REST API, özetleme kapsayıcı, tanım veya atamaya göre gerçekleştirilebilir. Azure Policy Insight 'ın abonelik Özeti kullanılarak abonelik düzeyinde [özetlemeye](/rest/api/policy-insights/policystates/summarizeforsubscription)bir örnek aşağıda verilmiştir:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2019-10-01
```

Çıktı, aboneliği özetler. Aşağıdaki örnek çıktıda, özetlenen uyumluluk **değer. Results., uyumsuz Antresources** ve **Value. Results. Nonkarmaşıkolmayan bir ilke**. Bu istek, her atamaya yönelik uyumlu olmayan numaraları ve tanım bilgilerini oluşturan her atama dahil olmak üzere daha fazla ayrıntı sağlar. Hiyerarşideki her ilke nesnesi, bu düzeyde ek ayrıntı almak için kullanılabilecek bir **Queryresultsurı** sağlar.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary/$entity",
        "results": {
            "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2019-10-01&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false",
            "nonCompliantResources": 15,
            "nonCompliantPolicies": 1
        },
        "policyAssignments": [{
            "policyAssignmentId": "/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77",
            "policySetDefinitionId": "",
            "results": {
                "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2019-10-01&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77'",
                "nonCompliantResources": 15,
                "nonCompliantPolicies": 1
            },
            "policyDefinitions": [{
                "policyDefinitionReferenceId": "",
                "policyDefinitionId": "/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "effect": "deny",
                "results": {
                    "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2019-10-01&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'",
                    "nonCompliantResources": 15
                }
            }]
        }]
    }]
}
```

### <a name="query-for-resources"></a>Kaynaklar için sorgu

Yukarıdaki örnekte **Value. Policyaslamalar. policyDefinitions. Results. queryResultsUri** , belirli bir ilke tanımı için uyumlu olmayan tüm kaynaklar için örnek bir URI sağlar. **$Filter** değerine bakarak, ısuyumlu değeri (EQ) false olarak, Policyassignmentıd, ilke tanımı için belirtilir ve sonra Policydefinitionıd 'nin kendisi için belirlenir. Filtrede Policyassignmentıd ekleme nedeni, Policydefinitionıd 'nin farklı kapsamlarla çeşitli ilkede veya girişim atamalarında mevcut olmasından kaynaklanır. Hem Policyassignmentıd hem de Policydefinitionıd belirterek aradığımız sonuçlara açık bir şekilde olabiliriz. Daha önce, PolicyStates için son **24 saatin bir** **Kimden** ve zaman penceresini otomatik olarak ayarlayan **en son**kullandık.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2019-10-01&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Aşağıdaki örnek yanıt, kısaltma için uyumlu olmayan tek bir kaynağa kırpılmıştır. Ayrıntılı yanıtta, kaynak, ilke veya girişim ve atama hakkında çeşitli veri parçaları bulunur. İlke tanımına hangi atama parametrelerinin geçtiğini de görebileceğinize dikkat edin.

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

Bir kaynak oluşturulduğunda veya güncelleştirilirken bir ilke değerlendirme sonucu oluşturulur. Sonuçlara _ilke olayları_denir. Abonelikle ilişkili en son ilke olaylarını görüntülemek için aşağıdaki URI 'yi kullanın.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2019-10-01
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

İlke olaylarını sorgulama hakkında daha fazla bilgi için bkz. [Azure Ilke olayları](/rest/api/policy-insights/policyevents) başvuru makalesi.

### <a name="azure-powershell"></a>Azure PowerShell

Azure Ilkesi için Azure PowerShell modülü, PowerShell Galerisi [az. Poliyghts](https://www.powershellgallery.com/packages/Az.PolicyInsights)olarak kullanılabilir.
PowerShellGet kullanarak modülünü kullanarak yükleyebilirsiniz `Install-Module -Name Az.PolicyInsights` (en son [Azure PowerShell](/powershell/azure/install-az-ps) yüklü olduğundan emin olun):

```azurepowershell-interactive
# Install from PowerShell Gallery via PowerShellGet
Install-Module -Name Az.PolicyInsights

# Import the downloaded module
Import-Module Az.PolicyInsights

# Login with Connect-AzAccount if not using Cloud Shell
Connect-AzAccount
```

Modül aşağıdaki cmdlet 'lere sahiptir:

- `Get-AzPolicyStateSummary`
- `Get-AzPolicyState`
- `Get-AzPolicyEvent`
- `Get-AzPolicyRemediation`
- `Remove-AzPolicyRemediation`
- `Start-AzPolicyRemediation`
- `Stop-AzPolicyRemediation`

Örnek: en yüksek sayıda uyumsuz kaynağa sahip en üstteki atanan ilkeye ait durum özeti alınıyor.

```azurepowershell-interactive
PS> Get-AzPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Örnek: en son değerlendirilen kaynağın durum kaydını alma (varsayılan değer zaman damgasına göre azalan sırada).

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

Örnek: uyumlu olmayan tüm sanal ağ kaynaklarının ayrıntılarını alma.

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

Örnek: belirli bir tarihten sonra gerçekleşen, uyumlu olmayan sanal ağ kaynaklarıyla ilgili olayları alma.

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

Azure PowerShell cmdlet 'ini kullanarak belirli bir kullanıcıyı almak için, **prenıd** alanı kullanılabilir `Get-AzADUser` . **{Prenaloıd}** değerini, önceki örnekte aldığınız Yanıtla değiştirin.

```azurepowershell-interactive
PS> (Get-AzADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Aboneliğinize bağlı Etkinlik Günlüğü Analizi çözümden sahip bir [Log Analytics çalışma alanınız](../../../log-analytics/log-analytics-overview.md) varsa `AzureActivity` , basit kusto sorguları ve tablosunu kullanarak, uyumluluk döngüsünden uyumsuzluk sonuçlarını da görüntüleyebilirsiniz [Activity Log Analytics solution](../../../azure-monitor/platform/activity-log-collect.md) `AzureActivity` . Azure Izleyici günlüklerindeki Ayrıntılar sayesinde, uyarılar uyumsuzluk izlemek üzere yapılandırılabilir.

:::image type="content" source="../media/getting-compliance-data/compliance-loganalytics.png" alt-text="Azure Izleyici günlüklerini kullanarak Azure Ilke uyumluluğu" border="false":::

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](programmatically-create.md)anlayın.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.