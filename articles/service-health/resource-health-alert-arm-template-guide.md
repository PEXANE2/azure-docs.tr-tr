---
title: Kaynak Durumu uyarıları oluşturmak için şablon
description: Azure kaynaklarınız kullanılamadığında sizi bilgilendiren programlı uyarılar oluşturun.
ms.topic: conceptual
ms.date: 9/4/2018
ms.openlocfilehash: 60ff5bdf2f4f0dab94c18fd7c751869c1893ad65
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759007"
---
# <a name="configure-resource-health-alerts-using-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarını kullanarak kaynak durumu uyarılarını yapılandırma

Bu makalede, Azure Kaynak Yöneticisi şablonları ve Azure PowerShell kullanarak programlı olarak Kaynak Durumu Etkinliği Günlüğü Uyarıları nasıl oluşturulacağı gösterecektir.

Azure Kaynak Durumu, Azure kaynaklarınızın geçerli ve geçmiş sağlık durumu hakkında sizi bilgilendirir. Azure Kaynak Durumu uyarıları, bu kaynakların sistem durumu durumlarında bir değişiklik olduğunda sizi neredeyse gerçek zamanlı olarak bilgilendirebilir. Kaynak Durumu uyarıları oluşturma, kullanıcıların toplu olarak uyarıları oluşturmasına ve özelleştirmesine olanak sağlar.

> [!NOTE]
> Kaynak Durumu uyarıları şu anda önizlemede.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu sayfadaki yönergeleri izlemek için birkaç şeyi önceden ayarlamanız gerekir:

1. [Azure PowerShell modüllerini](https://docs.microsoft.com/powershell/azure/install-Az-ps) yüklemeniz gerekir
2. Sizi bilgilendirmek üzere yapılandırılan [bir Eylem Grubu oluşturmanız veya yeniden kullanmanız](../azure-monitor/platform/action-groups.md) gerekir

## <a name="instructions"></a>Yönergeler
1. PowerShell'i kullanarak hesabınızı kullanarak Azure'da oturum açın ve etkileşimde kalmak istediğiniz aboneliği seçin

        Login-AzAccount
        Select-AzSubscription -Subscription <subscriptionId>

    > Erişiminiz `Get-AzSubscription` olan abonelikleri listelemek için kullanabilirsiniz.

2. Eylem Grubunuz için tam Azure Kaynak Yöneticisi Kimliğini bulun ve kaydedin

        (Get-AzActionGroup -ResourceGroupName <resourceGroup> -Name <actionGroup>).Id

3. Kaynak Durumu uyarıları için Kaynak Yöneticisi şablonu oluşturun ve kaydedin `resourcehealthalert.json` ( aşağıdaki[ayrıntılara bakın](#resource-manager-template-options-for-resource-health-alerts))

4. Bu şablonu kullanarak yeni bir Azure Kaynak Yöneticisi dağıtımı oluşturma

        New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <resourceGroup> -TemplateFile <path\to\resourcehealthalert.json>

5. Daha önce kopyaladığınız Uyarı Adı ve Eylem Grubu Kaynak Kimliğini yazmanız istenir:

        Supply values for the following parameters:
        (Type !? for Help.)
        activityLogAlertName: <Alert Name>
        actionGroupResourceId: /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/microsoft.insights/actionGroups/<actionGroup>

6. Her şey başarılı bir şekilde çalıştıysa, PowerShell'de onay alırsınız

        DeploymentName          : ExampleDeployment
        ResourceGroupName       : <resourceGroup>
        ProvisioningState       : Succeeded
        Timestamp               : 11/8/2017 2:32:00 AM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                                Name                     Type       Value
                                ===============          =========  ==========
                                activityLogAlertName     String     <Alert Name>
                                activityLogAlertEnabled  Bool       True
                                actionGroupResourceId    String     /...
        
        Outputs                 :
        DeploymentDebugLogLevel :

Bu işlemi tam olarak otomatiklendirmeyi planlıyorsanız, Adım 5'teki değerleri istenmeyecek şekilde Kaynak Yöneticisi şablonunu düzeltmeniz gerektiğini unutmayın.

## <a name="resource-manager-template-options-for-resource-health-alerts"></a>Kaynak Durumu uyarıları için Kaynak Yöneticisi şablon seçenekleri

Bu temel şablonu Kaynak Durumu uyarıları oluşturmak için başlangıç noktası olarak kullanabilirsiniz. Bu şablon yazılı olarak çalışır ve bir abonelikteki tüm kaynaklarda yeni etkinleştirilen tüm kaynak durumu olayları için uyarılar almak için kaydolur.

> Bu makalenin alt kısmında, kaynak durumu uyarıları için sinyali bu şablona kıyasla gürültü oranına yükseltecek daha karmaşık bir uyarı şablonu da eklenmiştir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": true,
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "ResourceHealth"
            },
            {
              "field": "status",
              "equals": "Active"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```

Ancak, bu gibi geniş bir uyarı genellikle tavsiye edilmez. Aşağıda önemsediğimiz olaylara odaklanmak için bu uyarıyı nasıl indirebileceğimizi öğrenin.

### <a name="adjusting-the-alert-scope"></a>Uyarı kapsamını ayarlama

Kaynak Durumu uyarıları, olayları üç farklı kapsamda izlemek üzere yapılandırılabilir:

 * Abonelik Düzeyi
 * Kaynak Grubu Düzeyi
 * Kaynak Düzeyi

Uyarı şablonu abonelik düzeyinde yapılandırılır, ancak uyarınızı yalnızca belirli kaynaklar veya belirli bir kaynak grubundaki kaynaklar hakkında sizi bilgilendirmek `scopes` üzere yapılandırmak istiyorsanız, yukarıdaki şablondaki bölümü değiştirmeniz yeterlidir.

Kaynak grubu düzeyi kapsamı için kapsamlar bölümü aşağıdaki gibi görünmelidir:
```json
"scopes": [
    "/subscriptions/<subscription id>/resourcegroups/<resource group>"
],
```

Ve kaynak düzeyi kapsamı için kapsam bölümü aşağıdaki gibi görünmelidir:

```json
"scopes": [
    "/subscriptions/<subscription id>/resourcegroups/<resource group>/providers/<resource>"
],
```

Örneğin, `"/subscriptions/d37urb3e-ed41-4670-9c19-02a1d2808ff9/resourcegroups/myRG/providers/microsoft.compute/virtualmachines/myVm"`

> Bu dizeyi elde etmek için Azure kaynağınızı görüntülerken Azure Portalı'na gidebilir ve URL'ye bakabilirsiniz.

### <a name="adjusting-the-resource-types-which-alert-you"></a>Sizi uyaran kaynak türlerini ayarlama

Abonelik veya kaynak grubu düzeyindeki uyarıların farklı türde kaynakları olabilir. Uyarıları yalnızca kaynak türlerinin belirli bir alt kümesinden gelenlerle sınırlamak istiyorsanız, `condition` şablonun aşağıdaki gibi bir bölümünde bunu tanımlayabilirsiniz:

```json
"condition": {
    "allOf": [
        ...,
        {
            "anyOf": [
                {
                    "field": "resourceType",
                    "equals": "MICROSOFT.COMPUTE/VIRTUALMACHINES",
                    "containsAny": null
                },
                {
                    "field": "resourceType",
                    "equals": "MICROSOFT.STORAGE/STORAGEACCOUNTS",
                    "containsAny": null
                },
                ...
            ]
        }
    ]
},
```

`anyOf` Burada, kaynak durumu uyarısının belirttiğimiz koşullardan herhangi birini eşleştirmesine izin vermek için sarıcıyı kullanırız ve belirli kaynak türlerini hedefleyen uyarılara izin veririz.

### <a name="adjusting-the-resource-health-events-that-alert-you"></a>Sizi uyaran Kaynak Durumu olaylarını ayarlama
Kaynaklar bir sağlık olayından geçtiğinde, sağlık olayının durumunu temsil eden bir `Active` `In Progress`dizi `Updated`aşamadan geçebilirler: , , ve `Resolved`.

Yalnızca bir kaynak sağlıksız hale geldiğinde haberdar olmak isteyebilirsiniz, bu durumda uyarınızı yalnızca . `status` `Active` Ancak, diğer aşamalarda da bilgilendirilmek istiyorsanız, aşağıdaki gibi ayrıntıları ekleyebilirsiniz:

```json
"condition": {
    "allOf": [
        ...,
        {
            "anyOf": [
                {
                    "field": "status",
                    "equals": "Active"
                },
                {
                    "field": "status",
                    "equals": "In Progress"
                },
                {
                    "field": "status",
                    "equals": "Resolved"
                },
                {
                    "field": "status",
                    "equals": "Updated"
                }
            ]
        }
    ]
}
```

Sağlık olaylarının dört aşaması için de bilgilendirilmek istiyorsanız, bu durumu hep birlikte kaldırabilirsiniz ve `status` uyarı özelliğinden bağımsız olarak sizi bilgilendirecektir.

> [!NOTE]
> Her "anyOf" bölümü yalnızca bir alan türü değerleri içermelidir.

### <a name="adjusting-the-resource-health-alerts-to-avoid-unknown-events"></a>"Bilinmeyen" olayları önlemek için Kaynak Durumu uyarılarını ayarlama

Azure Kaynak Durumu, test koşucularını kullanarak sürekli olarak izleyerek kaynaklarınızın en son sağlık durumunu size bildirebilir. Bildirilen ilgili sağlık durumları şunlardır: "Kullanılabilir", "Kullanılamıyor", ve "Bozulmuş". Ancak, koşucu ve Azure kaynağının iletişim kuramadığı durumlarda, kaynak için "Bilinmeyen" bir sağlık durumu bildirilir ve bu bir "Etkin" sistem durumu olarak kabul edilir.

Ancak, bir kaynak "Bilinmiyor" bildirdiğinde, son doğru rapordan bu yana sistem durumu değişmemiş olabilir. "Bilinmeyen" olaylardaki uyarıları ortadan kaldırmak istiyorsanız, şablondaki mantığı belirtebilirsiniz:

```json
"condition": {
    "allOf": [
        ...,
        {
            "anyOf": [
                {
                    "field": "properties.currentHealthStatus",
                    "equals": "Available",
                    "containsAny": null
                },
                {
                    "field": "properties.currentHealthStatus",
                    "equals": "Unavailable",
                    "containsAny": null
                },
                {
                    "field": "properties.currentHealthStatus",
                    "equals": "Degraded",
                    "containsAny": null
                }
            ]
        },
        {
            "anyOf": [
                {
                    "field": "properties.previousHealthStatus",
                    "equals": "Available",
                    "containsAny": null
                },
                {
                    "field": "properties.previousHealthStatus",
                    "equals": "Unavailable",
                    "containsAny": null
                },
                {
                    "field": "properties.previousHealthStatus",
                    "equals": "Degraded",
                    "containsAny": null
                }
            ]
        },
    ]
},
```

Bu örnekte, yalnızca geçerli ve önceki sağlık durumunun "Bilinmiyor" olmadığı olayları bildiriyoruz. Uyarılarınız doğrudan cep telefonunuza veya e-postanıza gönderilirse, bu değişiklik yararlı bir ek olabilir. 

GeçerliHealthStatus ve öncekiHealthStatus özelliklerinin bazı olaylarda null olmasının mümkün olduğunu unutmayın. Örneğin, güncelleştirilmiş bir olay oluştuğunda, kaynağın sistem durumu durumunun son rapordan bu yana değişmemiş olması, yalnızca ek olay bilgilerinin kullanılabilmesi (örn. neden). Bu nedenle, yukarıdaki yan tümceyi kullanmak bazı uyarıların tetiklenmemesine neden olabilir, çünkü özellikler.currentHealthStatus ve properties.previousHealthStatus değerleri null olarak ayarlanır.

### <a name="adjusting-the-alert-to-avoid-user-initiated-events"></a>Kullanıcı Tarafından Başlatılan olayları önlemek için uyarıyı ayarlama

Kaynak Durumu olayları platform tarafından tetiklenebilir başlatılan ve kullanıcı tarafından başlatılan olaylar. Yalnızca sistem durumu olayına Azure platformu neden olduğunda bildirim göndermek mantıklı olabilir.

Uyarınızı yalnızca bu tür olaylar için filtre uygulayacak şekilde yapılandırmak kolaydır:

```json
"condition": {
    "allOf": [
        ...,
        {
            "field": "properties.cause",
            "equals": "PlatformInitiated",
            "containsAny": null
        }
    ]
}
```
Bazı olaylarda neden alanının null olmasının mümkün olduğunu unutmayın. Diğer bir anda, bir sistem durumu geçişi gerçekleşir (örn. kullanılamıyor) ve bildirim gecikmelerini önlemek için olay hemen günlüğe kaydedilir. Bu nedenle, yukarıdaki yan tümcenin kullanılması, properties.clause özellik değeri null olarak ayarlanacağı için bir uyarının tetiklenmemesi ile sonuçlanabilir.

## <a name="complete-resource-health-alert-template"></a>Kaynak Durumu uyarısı şablonunu tamamla

Önceki bölümde açıklanan farklı ayarlamaları kullanarak, burada gürültü oranı sinyal maksimize etmek için yapılandırılan bir örnek şablon. GeçerliHealthStatus, previousHealthStatus ve neden özellik değerlerinin bazı olaylarda geçersiz olabileceği yukarıda belirtilen uyarıları unutmayın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "activityLogAlertName": {
            "type": "string",
            "metadata": {
                "description": "Unique name (within the Resource Group) for the Activity log alert."
            }
        },
        "actionGroupResourceId": {
            "type": "string",
            "metadata": {
                "description": "Resource Id for the Action group."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Insights/activityLogAlerts",
            "apiVersion": "2017-04-01",
            "name": "[parameters('activityLogAlertName')]",
            "location": "Global",
            "properties": {
                "enabled": true,
                "scopes": [
                    "[subscription().id]"
                ],
                "condition": {
                    "allOf": [
                        {
                            "field": "category",
                            "equals": "ResourceHealth",
                            "containsAny": null
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "properties.currentHealthStatus",
                                    "equals": "Available",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.currentHealthStatus",
                                    "equals": "Unavailable",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.currentHealthStatus",
                                    "equals": "Degraded",
                                    "containsAny": null
                                }
                            ]
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "properties.previousHealthStatus",
                                    "equals": "Available",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.previousHealthStatus",
                                    "equals": "Unavailable",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.previousHealthStatus",
                                    "equals": "Degraded",
                                    "containsAny": null
                                }
                            ]
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "properties.cause",
                                    "equals": "PlatformInitiated",
                                    "containsAny": null
                                }
                            ]
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "status",
                                    "equals": "Active",
                                    "containsAny": null
                                },
                                {
                                    "field": "status",
                                    "equals": "Resolved",
                                    "containsAny": null
                                },
                                {
                                    "field": "status",
                                    "equals": "In Progress",
                                    "containsAny": null
                                },
                                {
                                    "field": "status",
                                    "equals": "Updated",
                                    "containsAny": null
                                }
                            ]
                        }
                    ]
                },
                "actions": {
                    "actionGroups": [
                        {
                            "actionGroupId": "[parameters('actionGroupResourceId')]"
                        }
                    ]
                }
            }
        }
    ]
}
```

Ancak, hangi yapılandırmaların sizin için etkili olduğunu en iyi siz bileceksiniz, bu nedenle kendi özelleştirmenizi yapmak için bu belgelerde size öğretilen araçları kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak Durumu hakkında daha fazla bilgi edinin:
-  [Azure Kaynak Durumu'na genel bakış](Resource-health-overview.md)
-  [Azure Kaynak Durumu aracılığıyla kullanılabilen kaynak türleri ve durum denetimleri](resource-health-checks-resource-types.md)


Hizmet Durumu Uyarıları Oluşturun:
-  [Hizmet Durumu Için Uyarıları Yapılandırma](../azure-monitor/platform/alerts-activity-log-service-notifications.md) 
-  [Azure Etkinlik Günlüğü etkinlik şeması](../azure-monitor/platform/activity-log-schema.md)
