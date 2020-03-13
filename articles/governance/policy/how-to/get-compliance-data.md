---
title: İlke uyumluluk verilerini al
description: Azure İlkesi değerlendirmeleri ve etkileri uyumluluğunu belirler. Azure kaynaklarınızın uyumluluk ayrıntılarını nasıl alabileceğinizi öğrenin.
ms.date: 02/01/2019
ms.topic: how-to
ms.openlocfilehash: 891c9c72d8e83dc8f9adb930e8ebd11b70f6aad8
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79280644"
---
# <a name="get-compliance-data-of-azure-resources"></a>Azure kaynaklarının uyumluluk verilerini alın

Azure Ilkesinin en büyük avantajlarından biri, bir abonelik ya da bir abonelik [Yönetim grubundaki](../../management-groups/overview.md) kaynaklar üzerinde sağladığı Öngörüler ve denetimlerdir. Bu denetim, yanlış konumda oluşturulan kaynaklarını genel ve tutarlı etiket kullanım zorlamayı engelleyen gibi birçok farklı şekillerde uygulanabilecek veya yapılandırmaları ve ayarları denetim mevcut kaynakları için uygun. Her durumda, ortamınız uyumluluk durumunu anlamanıza olanak tanımak için Azure Ilkesi tarafından veriler oluşturulur.

Girişim atamaları ve ilke tarafından oluşturulan uyumluluk bilgileri erişmek için çeşitli yollar vardır:

- [Azure Portal](#portal) kullanma
- [Komut satırı](#command-line) komut dosyası aracılığıyla

Uyumluluk üzerinde yöntemleri bakarak önce uyumluluk bilgilerini güncelleştirildiğinde ve sıklığı ve değerlendirme döngüsü tetikleyen olayları göz atalım.

> [!WARNING]
> Uyumluluk durumu **kayıtlı değil**olarak bildiriliyorsa, **Microsoft. Policınsıghts** kaynak sağlayıcısının kayıtlı olduğunu ve kullanıcının [Azure ilkesinde RBAC](../overview.md#rbac-permissions-in-azure-policy)'de açıklandığı şekilde uygun rol tabanlı erişim denetimi (RBAC) izinlerine sahip olduğunu doğrulayın.

## <a name="evaluation-triggers"></a>Değerlendirme Tetikleyicileri

Tamamlanmış bir değerlendirme döngüsünün sonuçları, `PolicyStates` ve `PolicyEvents` işlemleri aracılığıyla `Microsoft.PolicyInsights` kaynak sağlayıcısında kullanılabilir. Azure Policy Insights REST API işlemleri hakkında daha fazla bilgi için bkz. [Azure Ilke öngörüleri](/rest/api/policy-insights/).

Atanan ilkeleri ve girişimler değerlendirmeleri çeşitli olayları sonucu olarak ortaya çıkar:

- Yeni bir ilke veya girişim bir kapsama atanmış. Tanımlanan kapsamına uygulanacak ataması için yaklaşık 30 dakika sürer. Bunu uygulandıktan sonra yeni atanan ilke veya girişim ve ilke tarafından kullanılan etkilerine bağlı olarak, kapsamı içindeki kaynaklar için değerlendirme döngüsü başlatır veya girişim, kaynaklar uyumlu veya uyumsuz olarak işaretlenir. Bir ilke veya girişim kaynaklarının büyük bir kapsam karşı değerlendirilir zaman alabilir. Bu nedenle, değerlendirme döngüsü tamamlanır, önceden tanımlanmış hiçbir beklentisi yoktur. İşlem tamamlandıktan sonra güncelleştirilmiş uyumluluk sonuçları portal ve SDK'ları kullanılabilir.

- Bir ilke veya girişim bir kapsama atanmış güncelleştirilir. Bu senaryo için zamanlama ve değerlendirme döngüsü aynıdır bir kapsam için yeni bir atama.

- Bir kaynak atama Resource Manager, REST, Azure CLI veya Azure PowerShell aracılığıyla bir kapsamla dağıtılır. Bu senaryoda, geçerli olay (ekleme, Denetim, reddetme, dağıtım) ve tek tek kaynak için uyumlu durumu bilgileri kullanılabilir portal ve SDK'ları yaklaşık 15 dakika sonra. Bu olay, bir değerlendirme diğer kaynakların neden olmaz.

- Standart uyumluluk değerlendirme döngüsü. Her 24 saatte bir kez atamaları otomatik olarak yeniden hesaplanır. Bu yüzden zaman değerlendirme döngüsünü, önceden tanımlanmış hiçbir beklentisi tamamlayacak bir ilke veya girişim birçok kaynak zaman alabilir. İşlem tamamlandıktan sonra güncelleştirilmiş uyumluluk sonuçları portal ve SDK'ları kullanılabilir.

- [Konuk yapılandırma](../concepts/guest-configuration.md) kaynak sağlayıcısı, yönetilen bir kaynak tarafından uyumluluk ayrıntılarıyla güncelleştirildi.

- İsteğe bağlı tarama

### <a name="on-demand-evaluation-scan"></a>İsteğe bağlı değerlendirme taraması

Bir abonelik veya kaynak grubu için bir değerlendirme taraması, REST API çağrısı ile başlatılabilir. Bu tarama zaman uyumsuz bir işlemdir. Bu nedenle, tarama yanıt tamamlanana kadar tarama başlatmak için REST uç noktasını beklemez. Bunun yerine, istenen değerlendirme durumunu sorgulamak için bir URI sağlar.

Her bir REST API URI'sinde kendi değerlerinizle değiştirmeniz gereken değişkenler bulunur:

- `{YourRG}`-kaynak grubunuzun adıyla değiştirin
- `{subscriptionId}` - Abonelik kimliğinizle değiştirin

Tarama, bir abonelik veya kaynak grubundaki kaynakların değerlendirme destekler. Aşağıdaki URI yapılarını kullanarak bir REST API **Post** komutuyla kapsama göre tarama başlatın:

- Abonelik

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

- Kaynak grubu

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{YourRG}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

Çağrı, **202 kabul edilen** bir durum döndürür. Yanıt üst bilgisine eklenen bir **konum** özelliği aşağıdaki biçimdedir:

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/asyncOperationResults/{ResourceContainerGUID}?api-version=2018-07-01-preview
```

`{ResourceContainerGUID}`, istenen kapsam için statik olarak oluşturulur. Bir kapsam bir isteğe bağlı tarama zaten çalışıyorsa, yeni bir tarama başlatıldı değil. Bunun yerine, yeni istek durum için aynı `{ResourceContainerGUID}` **konum** URI 'si olarak sağlanır. **Konum** URI 'sine yönelik bir REST API **Get** komutu, değerlendirme devam ederken **kabul edilen bir 202** döndürür. Değerlendirme taraması tamamlandığında, **200 bir Tamam** durumu döndürür. Tamamlanmış bir tarama durumuyla birlikte bir JSON yanıt gövdesidir:

```json
{
    "status": "Succeeded"
}
```

## <a name="how-compliance-works"></a>Uyumluluk nasıl çalışır?

Bir atamada, ilke veya girişim kurallarını takip etmez bir kaynak **uyumlu** değildir.
Aşağıdaki tabloda, farklı ilke efektler elde edilen uyumluluk durumu için Koşul değerlendirmesi ile çalışma gösterilmektedir:

| Kaynak durumu | Etki | İlke değerlendirmesi | Uyumluluk durumu |
| --- | --- | --- | --- |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Uyumlu değil |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | True | Uyumlu değil |
| Yeni | Audit, AuditIfNotExist\* | False | Uyumlu |

\* Append, DeployIfNotExist ve AuditIfNotExist etkileri IF deyiminin TRUE olmasını gerektirir.
Etkiler ayrıca varlık koşulunun uyumlu olmaması için FALSE olmasını gerektirir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

Örneğin, ortak ağlara sunulan bazı depolama hesapları (kırmızı renkte vurgulanmış) ile bir kaynak grubu – ContsoRG, olduğunu varsaymaktadır.

![Ortak ağlara maruz depolama hesapları](../media/getting-compliance-data/resource-group01.png)

Bu örnekte, güvenlik risklerini dikkatli olmanız gerekir. Bir ilke ataması oluşturduğunuza göre ContosoRG kaynak grubundaki tüm depolama hesapları için değerlendirilir. Bu, uyumlu olmayan üç depolama hesabını denetler, sonuç olarak durumlarını **uyumlu değil** olarak değiştirir.

![Uyumlu olmayan depolama hesapları denetlendi](../media/getting-compliance-data/resource-group03.png)

**Uyumlu** ve **uyumlu olmayan**, ilkelerin ve kaynakların yanında üç farklı durum vardır:

- **Çakışıyor**: çakışan kurallara sahip iki veya daha fazla ilke var. Örneğin, iki ilke aynı etiketi farklı değerlerle ekleme.
- **Başlamadı**: ilke veya kaynak için değerlendirme çevrimi başlatılmadı.
- **Kayıtlı değil**: Azure Ilke kaynak sağlayıcısı kayıtlı değil veya oturum açmış hesabın uyumluluk verilerini okuma izni yok.

Azure Ilkesi, bir kaynağın eşleşme olup olmadığını anlamak için tanımdaki **tür** ve **ad** alanlarını kullanır. Kaynak eşleştiğinde, geçerli kabul edilir ve durumu **uyumlu** veya **uyumsuz**olarak değerlendirilir. Her iki **tür** veya **ad** tanımdaki tek özelliktir, tüm kaynaklar uygulanabilir kabul edilir ve değerlendirilir.

Uyumluluk yüzdesi, **uyumlu** kaynakları _Toplam kaynağa_bölerek belirlenir.
_Toplam kaynak_ , **uyumlu**, **uyumlu olmayan**ve **Çakışan** kaynakların toplamı olarak tanımlanır. Genel uyumluluk numaraları, tüm ayrı kaynakların **toplamına ayrılan ayrı** kaynakların toplamıdır. Aşağıdaki görüntüde, uygulanabilir ve yalnızca bir tane **uyumlu olmayan**20 farklı kaynak vardır. Genel kaynak uyumluluk % 95'inden (19 / 20) ' dir.

![Uyumluluk sayfasından ilke uyumluluğu örneği](../media/getting-compliance-data/simple-compliance.png)

## <a name="portal"></a>Portal

Azure portalında bir grafik deneyimi Görselleştirme ve anlama ortamınızın uyumluluk durumunu gösterir. **İlke** sayfasında **genel bakış** seçeneği, hem ilkelerin hem de girişimlerin uyumluluğuyla kullanılabilir kapsamlar için ayrıntılar sağlar. Uyumluluk durumu ve başına atama sayısı ile birlikte, son yedi güne uyumluluk gösteren bir grafiği içerir. **Uyumluluk** sayfası, bu bilgilerin büyük bölümünü (grafik dışında) içerir, ancak ek filtreleme ve sıralama seçenekleri sağlar.

![Azure Ilkesi uyumluluk sayfası örneği](../media/getting-compliance-data/compliance-page.png)

Bir ilke veya girişim farklı kapsamlara atanabilir olduğundan, tablo, her atama ve tür tanımının atandığı için kapsamı içerir. Uyumlu olmayan kaynakları ve her atama için uyumlu olmayan ilkeler de sağlanır. Bir ilke veya girişim tabloda tıklayarak bu atama için Uyumluluk, daha kapsamlı bir bakış sağlar.

![Azure Ilke uyumluluğu ayrıntıları sayfası örneği](../media/getting-compliance-data/compliance-details.png)

**Kaynak uyumluluğu** sekmesindeki kaynakların listesi, geçerli atama için mevcut kaynakların değerlendirme durumunu gösterir. Sekme varsayılan olarak **uyumlu değildir**, ancak filtrelenebilir.
Kaynak oluşturma isteği tarafından tetiklenen olaylar (Append, denetle, Reddet, dağıt), **Olaylar** sekmesi altında gösterilir.

> [!NOTE]
> AKS altyapı ilkesi için, gösterilen kaynak kaynak grubudur.

![Azure Ilkesi uyumluluk olayları örneği](../media/getting-compliance-data/compliance-events.png)

Kaynak [sağlayıcısı modu](../concepts/definition-structure.md#resource-provider-modes) kaynakları için kaynak **uyumluluğu** sekmesinde, kaynağı seçip satırı sağ tıklatıp **görünüm uyumluluk ayrıntıları** ' nı seçerek bileşen uyumluluk ayrıntılarını açılır. Bu sayfa, bu kaynağa, olaylara, bileşen olaylarına ve değişiklik geçmişine atanan ilkeleri görmek için de sekmeler sağlar.

![Azure Ilke bileşeni uyumluluk ayrıntıları örneği](../media/getting-compliance-data/compliance-components.png)

Kaynak uyumluluğu sayfasına dönün, daha fazla ayrıntı toplamak istediğiniz olayın satırına sağ tıklayın ve **etkinlik günlüklerini göster**' i seçin. Etkinlik günlüğü sayfasında açılır ve atama ve olayların ayrıntılarını gösteren arama önceden filtre uygulanmış. Etkinlik günlüğü ek bağlam ve bu olaylar hakkında bilgi sağlar.

![Azure Ilke uyumluluğu etkinlik günlüğü örneği](../media/getting-compliance-data/compliance-activitylog.png)

### <a name="understand-non-compliance"></a>Uyumsuzluğun anlaşılma

<a name="change-history-preview"></a>

Bir kaynağın **uyumsuz**olduğu belirlendiğinde birçok olası neden vardır. Kaynağın **uyumsuz** olma nedenini öğrenmek veya sorumluyu bulmak için bkz. [uyumsuzluğu belirleme](./determine-non-compliance.md).

## <a name="command-line"></a>Komut satırı

Portalda bulunan bilgiler REST API ( [Armclient](https://github.com/projectkudu/ARMClient)dahil), Azure PowerShell ve Azure CLI (Önizleme) ile birlikte alınabilir.
REST API hakkında tam Ayrıntılar için bkz. [Azure Policy Insights](/rest/api/policy-insights/) başvurusu. REST API başvuru sayfalarına deneyin olanak tanıyan her işlemi ''deneyin It bir yeşil düğmeyi sahip sağdaki tarayıcıdaki.

REST API örnekleri için Azure kimlik doğrulamasını işlemek üzere ARMClient veya benzer bir araç kullanın.

### <a name="summarize-results"></a>Sonuçlarını özetleme

REST API ile kapsayıcı, tanımını veya atamasını özetleme gerçekleştirilebilir. Azure Policy Insight 'ın abonelik Özeti kullanılarak abonelik düzeyinde [özetlemeye](/rest/api/policy-insights/policystates/summarizeforsubscription)bir örnek aşağıda verilmiştir:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04
```

Çıkış abonelik özetler. Aşağıdaki örnek çıktıda, özetlenen uyumluluk **değer. Results., uyumsuz Antresources** ve **Value. Results. Nonkarmaşıkolmayan bir ilke**. Bu istek hakkında daha fazla ayrıntı, uyumlu olmayan sayılar ve her bir atama için tanım bilgisi yapılan her atama dahil olmak üzere sağlar. Hiyerarşideki her ilke nesnesi, bu düzeyde ek ayrıntı almak için kullanılabilecek bir **Queryresultsurı** sağlar.

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

Yukarıdaki örnekte **Value. Policyaslamalar. policyDefinitions. Results. queryResultsUri** , belirli bir ilke tanımı için uyumlu olmayan tüm kaynaklar için örnek bir URI sağlar. **$Filter** değerine bakarak, ısuyumlu değeri (EQ) false olarak, Policyassignmentıd, ilke tanımı için belirtilir ve sonra Policydefinitionıd 'nin kendisi için belirlenir. Birden çok ilke veya girişim atamaları farklı kapsamlarla Policydefinitionıd var olabilir çünkü PolicyAssignmentId filtreye dahil olmak üzere nedenidir. PolicyAssignmentId hem Policydefinitionıd belirterek, biz bekliyoruz sonuçlarında açık olabilir. Daha önce, PolicyStates için son **24 saatin bir** **Kimden** ve zaman penceresini otomatik olarak ayarlayan **en son**kullandık.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Aşağıdaki örnek yanıt, tek bir uyumsuz kaynağı kısaltma için kırpılır. Ayrıntılı yanıt, veri kaynağı, ilke veya girişim ve atama hakkında birkaç bölümü ele alınmakta sahiptir. Ayrıca ilke tanımını atama parametreleri geçirilen gördüğünüzden dikkat edin.

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

Bir kaynak oluşturulduğunda veya güncelleştirildiğinde, ilke değerlendirme sonucu üretilir. Sonuçlara _ilke olayları_denir. Abonelikle ilişkili son ilke olaylarını görüntülemek için aşağıdaki URI'ı kullanın.

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

İlke olaylarını sorgulama hakkında daha fazla bilgi için bkz. [Azure Ilke olayları](/rest/api/policy-insights/policyevents) başvuru makalesi.

### <a name="azure-powershell"></a>Azure PowerShell

Azure Ilkesi için Azure PowerShell modülü, PowerShell Galerisi [az. Poliyghts](https://www.powershellgallery.com/packages/Az.PolicyInsights)olarak kullanılabilir.
PowerShellGet kullanarak modülü `Install-Module -Name Az.PolicyInsights` kullanarak yükleyebilirsiniz (en son [Azure PowerShell](/powershell/azure/install-az-ps) yüklü olduğundan emin olun):

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

Örnek: durumu için en üstteki atanan ilke ile uyumlu olmayan kaynakları en yüksek sayısını özeti alınıyor.

```azurepowershell-interactive
PS> Get-AzPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Örnek: en son kaynak değerlendirilmesi için durum kaydı alma (varsayılan değer azalan zaman damgası tarafından).

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

Örnek: tüm uyumlu sanal ağ kaynakları için Ayrıntılar alınıyor.

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

Örnek: belirli bir tarihten sonra oluştu uyumlu olmayan bir sanal ağ kaynakları ile ilgili olayları alınıyor.

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

`Get-AzADUser`Azure PowerShell cmdlet 'ini kullanarak belirli bir kullanıcıyı almak için **Prenaloıd** alanı kullanılabilir. **{Prenaloıd}** değerini, önceki örnekte aldığınız Yanıtla değiştirin.

```azurepowershell-interactive
PS> (Get-AzADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Aboneliğinize bağlı [etkinlik günlüğü Analizi çözümünden](../../../azure-monitor/platform/activity-log-collect.md) `AzureActivity` sahip bir [Log Analytics çalışma alanınız](../../../log-analytics/log-analytics-overview.md) varsa, ayrıca basit kusto sorguları ve `AzureActivity` tablosunu kullanarak, değerlendirme döngüsünden uyumsuzluk sonuçlarını görüntüleyebilirsiniz. Azure Izleyici günlüklerindeki Ayrıntılar sayesinde, uyarılar uyumsuzluk izlemek üzere yapılandırılabilir.


![Azure Izleyici günlüklerini kullanarak Azure Ilke uyumluluğu](../media/getting-compliance-data/compliance-loganalytics.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](programmatically-create.md)anlayın.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.