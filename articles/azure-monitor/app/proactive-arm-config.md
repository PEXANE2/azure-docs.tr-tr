---
title: Akıllı algılama kuralı ayarları - Azure Uygulama Öngörüleri
description: Azure Kaynak Yöneticisi Şablonları ile Azure Application Insights akıllı algılama kurallarının yönetimini ve yapılandırmanı otomatikleştirin
ms.topic: conceptual
author: harelbr
ms.author: harelbr
ms.date: 06/26/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 7ca4df620739b2ab55b8ba986031cc48fe87f1fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294911"
---
# <a name="manage-application-insights-smart-detection-rules-using-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak Uygulama Öngörüleri akıllı algılama kurallarını yönetme

Uygulama Öngörüleri'ndeki akıllı algılama kuralları [Azure Kaynak Yöneticisi şablonları](../../azure-resource-manager/templates/template-syntax.md)kullanılarak yönetilebilir ve yapılandırılabilir.
Bu yöntem, Azure Kaynak Yöneticisi otomasyonu yla yeni Application Insights kaynaklarını dağıtırken veya varolan kaynakların ayarlarını değiştirirken kullanılabilir.

## <a name="smart-detection-rule-configuration"></a>Akıllı algılama kuralı yapılandırması

Akıllı algılama kuralı için aşağıdaki ayarları yapılandırabilirsiniz:
- Kural etkinse (varsayılan **değer doğrudur.)**
- Bir algılama bulunduğunda aboneliğin [İzleme Okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) ve [İzleme Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rolleriyle ilişkili kullanıcılara e-postalar gönderilmesi gerekiyorsa (varsayılan **değer doğrudur**.)
- Bir algılama bulunduğunda bildirim alması gereken ek e-posta alıcıları.
    -  Önizleme _olarak_işaretlenmiş Akıllı Algılama kuralları için e-posta yapılandırması kullanılamıyor.

Kural ayarlarının Azure Kaynak Yöneticisi aracılığıyla yapılandırılsın, akıllı algılama kuralı yapılandırması artık Application Insights kaynağında **ProactiveDetectionConfigs**adlı bir iç kaynak olarak kullanılabilir.
Maksimum esneklik için, her akıllı algılama kuralı benzersiz bildirim ayarlarıyla yapılandırılabilir.

## <a name="examples"></a>Örnekler

Aşağıda, Azure Kaynak Yöneticisi şablonlarını kullanarak akıllı algılama kurallarının ayarlarını nasıl yapılandırılacakgösterilen birkaç örnek verilmiştir.
Tüm örnekler, _"myApplication"_ adlı bir Application Insights kaynağına ve dahili olarak _"uzun bağımlılık süresi"_ olarak adlandırılan "uzun bağımlılık süresi akıllı algılama kuralı"na başvurur.
Application Insights kaynak adını değiştirdiğinden ve ilgili akıllı algılama kuralıiç adını belirttiğinden emin olun. Her akıllı algılama kuralı için ilgili dahili Azure Kaynak Yöneticisi adlarının listesini aşağıdaki tabloya denetleyin.

### <a name="disable-a-smart-detection-rule"></a>Akıllı algılama kuralını devre dışı atma

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": [],
            "enabled": false
          }
        }
      ]
    }
```

### <a name="disable-sending-email-notifications-for-a-smart-detection-rule"></a>Akıllı algılama kuralı için e-posta bildirimlerini göndermeyi devre dışı

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": false,
            "customEmails": [],
            "enabled": true
          }
        }
      ]
    }
```

### <a name="add-additional-email-recipients-for-a-smart-detection-rule"></a>Akıllı algılama kuralı için ek e-posta alıcıları ekleme

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": ['alice@contoso.com', 'bob@contoso.com'],
            "enabled": true
          }
        }
      ]
    }

```


## <a name="smart-detection-rule-names"></a>Akıllı algılama kural adları

Aşağıda, portalda göründükleri gibi, Azure Kaynak Yöneticisi şablonunda kullanılması gereken dahili adlarıyla birlikte akıllı algılama kuralı adlarının tablosu verilmiştir.

> [!NOTE]
> _Önizleme_ olarak işaretlenmiş akıllı algılama kuralları e-posta bildirimlerini desteklemez. Bu nedenle, yalnızca bu kurallar için _etkin özelliği_ ayarlayabilirsiniz. 

| Azure portal kural adı | Dahili ad
|:---|:---|
| Yavaş sayfa yükleme süresi | slowpageloadtime |
| Yavaş sunucu yanıt süresi | slowserverresponsetime |
| Uzun bağımlılık süresi | uzun bağımlılık süresi |
| Sunucu yanıt süresinde bozulma | bozulmasunucu yanıt süresi |
| Bağımlılık süresinde bozulma | bozulmalar bağımlılık süresi |
| İz önem oranındaki bozulma (önizleme) | extension_traceseveritydetector |
| Özel durum hacminde anormal artış (önizleme) | extension_exceptionchangeextension |
| Olası bellek sızıntısı algılandı (önizleme) | extension_memoryleakextension |
| Olası güvenlik sorunu algılandı (önizleme) | extension_securityextensionspackage |
| Günlük veri hacminde anormal artış (önizleme) | extension_billingdatavolumedailyspikeextension |

### <a name="failure-anomalies-alert-rule"></a>Başarısızlık Anomalileri uyarı kuralı

Bu Azure Kaynak Yöneticisi şablonu, bir Hata Anomalileri uyarı kuralını 2 şiddetinde yapılandırıldığını gösterir. Hata Anomalileri uyarı kuralının bu yeni sürümü yeni Azure uyarı platformunun bir parçasıdır ve [klasik uyarılar emeklilik sürecinin](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/)bir parçası olarak kullanımdan kaldırılan klasik sürümün yerini alır.

> [!NOTE]
> Hata Anomalileri küresel bir hizmettir, bu nedenle kural konumu küresel konumda oluşturulur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "microsoft.alertsmanagement/smartdetectoralertrules",
            "apiVersion": "2019-03-01",
            "name": "Failure Anomalies - my-app",
            "location": "global", 
            "properties": {
                  "description": "Failure Anomalies notifies you of an unusual rise in the rate of failed HTTP requests or dependency calls.",
                  "state": "Enabled",
                  "severity": "2",
                  "frequency": "PT1M",
                  "detector": {
                  "id": "FailureAnomaliesDetector"
                  },
                  "scope": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/MyResourceGroup/providers/microsoft.insights/components/my-app"],
                  "actionGroups": {
                        "groupIds": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourcegroups/MyResourceGroup/providers/microsoft.insights/actiongroups/MyActionGroup"]
                  }
            }
        }
    ]
}
```

> [!NOTE]
> Bu Azure Kaynak Yöneticisi şablonu, Hata Anomalileri uyarı kuralına özgüdür ve bu makalede açıklanan diğer klasik Akıllı Algılama kurallarından farklıdır. Hata Anomalileri'ni el ile yönetmek istiyorsanız, bu işlem Azure Monitor Uyarıları'nda yapılırken, diğer tüm Akıllı Algılama kuralları UI'nin Akıllı Algılama bölmesinde yönetilir.

## <a name="next-steps"></a>Sonraki Adımlar

Otomatik olarak algılama hakkında daha fazla bilgi edinin:

- [Hata anormallikleri](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Bellek Sızıntıları](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Performans anomalileri](../../azure-monitor/app/proactive-performance-diagnostics.md)
