---
title: Yeni öneriler için Azure Danışmanı uyarıları oluşturun
description: Yeni öneri için Azure Danışmanı uyarıları oluşturun
ms.topic: article
ms.date: 09/09/2019
ms.openlocfilehash: 07cbc57ef718b6cac104d2b5238ff4e3196f197a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443165"
---
# <a name="create-azure-advisor-alerts-on-new-recommendations"></a>Yeni önerilerde Azure Danışmanı uyarıları oluşturma 

Bu makalede, Azure portalı ve Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Danışmanı'nın yeni önerileri için nasıl bir uyarı oluşturabileceğiniz gösterilmektedir. 

Azure Danışmanı kaynaklarınızdan biri için yeni bir öneri algıladığında, bir olay [Azure Etkinliği günlüğünde](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview)depolanır. Öneriye özel uyarılar oluşturma deneyimini kullanarak Azure Danışmanı'ndan bu etkinlikler için uyarılar ayarlayabilirsiniz. Uyarı almak istediğiniz kaynakları belirtmek için bir abonelik ve isteğe bağlı olarak bir kaynak grubu seçebilirsiniz. 

Bu özellikleri kullanarak da öneri türlerini belirleyebilirsiniz:

* Kategori
* Etki düzeyi
* Öneri türü

Ayrıca, bir uyarı tarafından tetiklendiğinde gerçekleşecek eylemi de yapılandırabilirsiniz:  

* Varolan bir eylem grubunu seçme
* Yeni bir eylem grubu oluşturma

Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

> [!NOTE] 
> Danışman uyarıları şu anda yalnızca Yüksek Kullanılabilirlik, Performans ve Maliyet önerileri için kullanılabilir. Güvenlik önerileri desteklenmez. 

## <a name="in-the-azure-portal"></a>Azure portalında
1. **Portalda**Azure **Danışmanı'nı**seçin.

    ![Portalda Azure Danışmanı](./media/advisor-alerts/create1.png)

2. Sol menünün **İzleme** bölümünde **Uyarılar'ı**seçin. 

    ![Danışman'daki Uyarılar](./media/advisor-alerts/create2.png)

3. **Yeni Danışman Uyarısı'nı**seçin.

    ![Yeni Danışman uyarısı](./media/advisor-alerts/create3.png)

4. **Kapsam** bölümünde, aboneliği ve isteğe bağlı olarak uyarı almak istediğiniz kaynak grubunu seçin. 

    ![Danışman uyarı kapsamı](./media/advisor-alerts/create4.png)

5. **Durum** bölümünde, uyarınızı yapılandırmak için kullanmak istediğiniz yöntemi seçin. Belirli bir kategori ve/veya etki düzeyi için tüm öneriler için uyarı yapmak istiyorsanız, **Kategori ve etki düzeyini**seçin. Belirli bir türdeki tüm öneriler için uyarı yapmak istiyorsanız, **Öneri türünü**seçin.

    ![Azure Danışmanı uyarı durumu](./media/advisor-alerts/create5.png)

6. Seçtiğiniz seçenegöre Yapılandır'a bağlı olarak, ölçütleri belirtebilirsiniz. Tüm önerileri istiyorsanız, kalan alanları boş bırakmanız gerekir. 

    ![Danışman uyarı eylem grubu](./media/advisor-alerts/create6.png)

7. Eylem **grupları** bölümünde, zaten oluşturduğunuz bir eylem grubunu kullanmak için **varolan ekle'yi** veya yeni bir [eylem grubu](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)kurmak için **yeni oluştur'u** seçin. 

    ![Danışman uyarısı varolan ekle](./media/advisor-alerts/create7.png)

8. Uyarı ayrıntıları bölümünde, uyarınıza bir ad ve kısa açıklama verin. Uyarınızın etkinleştirilmesini istiyorsanız, **oluşturma seçiminde** **Evet**olarak ayarlanan kuralı etkinleştir'i bırakın. Ardından uyarınızı kaydetmek için kaynak grubunu seçin. Bu, önerinin hedefleme kapsamını etkilemez. 

    ![Azure Danışman Banner](./media/advisor-alerts/create8.png)


## <a name="with-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonuyla

Bu Kaynak Yöneticisi şablonu bir öneri uyarısı ve yeni bir eylem grubu oluşturur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroups_name": {
      "defaultValue": "advisorAlert",
      "type": "string"
    },
    "activityLogAlerts_name": {
      "defaultValue": "AdvisorAlertsTest2",
      "type": "string"
    },
    "emailAddress": {
      "defaultValue": "<email address>",
      "type": "string"
    }
  },
  "variables": {
    "alertScope": "[concat('/','subscriptions','/',subscription().subscriptionId)]"
  },
  "resources": [
    {
      "comments": "Action Group",
      "type": "microsoft.insights/actionGroups",
      "name": "[parameters('actionGroups_name')]",
      "apiVersion": "2017-04-01",
      "location": "Global",
      "tags": {},
      "scale": null,
      "properties": {
        "groupShortName": "[parameters('actionGroups_name')]",
        "enabled": true,
        "emailReceivers": [
          {
            "name": "[parameters('actionGroups_name')]",
            "emailAddress": "[parameters('emailAddress')]"
          }
        ],
        "smsReceivers": [],
        "webhookReceivers": []
      },
      "dependsOn": []
    },
    {
      "comments": "Azure Advisor Activity Log Alert",
      "type": "microsoft.insights/activityLogAlerts",
      "name": "[parameters('activityLogAlerts_name')]",
      "apiVersion": "2017-04-01",
      "location": "Global",
      "tags": {},
      "scale": null,
      "properties": {
        "scopes": [
          "[variables('alertScope')]"
        ],
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Recommendation"
            },
            {
              "field": "properties.recommendationCategory",
              "equals": "Cost"
            },
            {
              "field": "properties.recommendationImpact",
              "equals": "Medium"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Advisor/recommendations/available/action"
            }
          ]
        },
        "actions": {
          "actionGroups": [
            {
              "actionGroupId": "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]",
              "webhookProperties": {}
            }
          ]
        },
        "enabled": true,
        "description": ""
      },
      "dependsOn": [
        "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]"
      ]
    }
  ]
}
  ```

## <a name="configure-recommendation-alerts-to-use-a-webhook"></a>Webhook kullanmak için öneri uyarılarını yapılandırma
Bu bölümde, öneri verilerini webhook'lar aracılığıyla mevcut sistemlerinize göndermek için Azure Danışmanı uyarılarını nasıl yapılandırabileceğiniz gösterilmektedir. 

Kaynaklarınızdan birinde yeni bir Danışman önerisi olduğunda, bildirimde bildirilecek uyarılar ayarlayabilirsiniz. Bu uyarılar sizi e-posta veya kısa mesaj yoluyla bilgilendirebilir, ancak bir webhook aracılığıyla mevcut sistemlerinizle tümleştirmek için de kullanılabilir. 


### <a name="using-the-advisor-recommendation-alert-payload"></a>Danışman önerisi uyarı yükünü kullanma
Bir webhook kullanarak Danışman uyarılarını kendi sistemlerinize entegre etmek istiyorsanız, bildirimden gönderilen JSON yükünü ayrıştırman gerekir. 

Eylem grubunuzu bu uyarı için ayarladığınızda, ortak uyarı şemasını kullanmak ister misiniz' seçeneğini belirlersiniz. Ortak uyarı şemasını seçerseniz, yükünüz aşağıdaki gibi görünür: 

```json
{  
   "schemaId":"azureMonitorCommonAlertSchema",
   "data":{  
      "essentials":{  
         "alertId":"/subscriptions/<subid>/providers/Microsoft.AlertsManagement/alerts/<alerted>",
         "alertRule":"Webhhook-test",
         "severity":"Sev4",
         "signalType":"Activity Log",
         "monitorCondition":"Fired",
         "monitoringService":"Activity Log - Recommendation",
         "alertTargetIDs":[  
            "/subscriptions/<subid>/resourcegroups/<resource group name>/providers/microsoft.dbformariadb/servers/<resource name>"
         ],
         "originAlertId":"001d8b40-5d41-4310-afd7-d65c9d4428ed",
         "firedDateTime":"2019-07-17T23:00:57.3858656Z",
         "description":"A new recommendation is available.",
         "essentialsVersion":"1.0",
         "alertContextVersion":"1.0"
      },
      "alertContext":{  
         "channels":"Operation",
         "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
         "caller":"Microsoft.Advisor",
         "correlationId":"8554b847-2a72-48ef-9776-600aca3c3aab",
         "eventSource":"Recommendation",
         "eventTimestamp":"2019-07-17T22:28:54.1566942+00:00",
         "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
         "eventDataId":"001d8b40-5d41-4310-afd7-d65c9d4428ed",
         "level":"Informational",
         "operationName":"Microsoft.Advisor/recommendations/available/action",
         "properties":{  
            "recommendationSchemaVersion":"1.0",
            "recommendationCategory":"Performance",
            "recommendationImpact":"Medium",
            "recommendationName":"Increase the MariaDB server vCores",
            "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/source/ActivityLog/recommendationTypeId/a5f888e3-8cf4-4491-b2ba-b120e14eb7ce/resourceId/%2Fsubscriptions%<subscription id>%2FresourceGroups%2<resource group name>%2Fproviders%2FMicrosoft.DBforMariaDB%2Fservers%2F<resource name>",
            "recommendationType":"a5f888e3-8cf4-4491-b2ba-b120e14eb7ce"
         },
         "status":"Active",
         "subStatus":"",
         "submissionTimestamp":"2019-07-17T22:28:54.1566942+00:00"
      }
   }
}
  ```

Ortak şemayı kullanmıyorsanız, yükünüz aşağıdaki gibi görünür: 

```json
{  
   "schemaId":"Microsoft.Insights/activityLogs",
   "data":{  
      "status":"Activated",
      "context":{  
         "activityLog":{  
            "channels":"Operation",
            "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
            "caller":"Microsoft.Advisor",
            "correlationId":"3ea7320f-c002-4062-adb8-96d3bd92a5f4",
            "description":"A new recommendation is available.",
            "eventSource":"Recommendation",
            "eventTimestamp":"2019-07-17T20:36:39.3966926+00:00",
            "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
            "eventDataId":"a12b8e59-0b1d-4003-bfdc-3d8152922e59",
            "level":"Informational",
            "operationName":"Microsoft.Advisor/recommendations/available/action",
            "properties":{  
               "recommendationSchemaVersion":"1.0",
               "recommendationCategory":"Performance",
               "recommendationImpact":"Medium",
               "recommendationName":"Increase the MariaDB server vCores",
               "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/source/ActivityLog/recommendationTypeId/a5f888e3-8cf4-4491-b2ba-b120e14eb7ce/resourceId/%2Fsubscriptions%2F<subscription id>%2FresourceGroups%2F<resource group name>%2Fproviders%2FMicrosoft.DBforMariaDB%2Fservers%2F<resource name>",
               "recommendationType":"a5f888e3-8cf4-4491-b2ba-b120e14eb7ce"
            },
            "resourceId":"/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/microsoft.dbformariadb/servers/<resource name>",
            "resourceGroupName":"<resource group name>",
            "resourceProviderName":"MICROSOFT.DBFORMARIADB",
            "status":"Active",
            "subStatus":"",
            "subscriptionId":"<subscription id>",
            "submissionTimestamp":"2019-07-17T20:36:39.3966926+00:00",
            "resourceType":"MICROSOFT.DBFORMARIADB/SERVERS"
         }
      },
      "properties":{  
 
      }
   }
}
```

Her iki şemada, **eventSource** ve `Recommendation` **operationName'i** arayarak `Microsoft.Advisor/recommendations/available/action`Danışman tavsiye olaylarını tanımlayabilirsiniz.

Kullanmak isteyebileceğiniz diğer önemli alanlardan bazıları şunlardır: 

* *alertTargetIDs* (ortak şemada) veya *resourceId* (eski şema)
* *recommendationType*
* *recommendationName*
* *tavsiyeKategori*
* *tavsiyeEtki*
* *tavsiyeResourceLink*


## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme 

Azure Advisor'dan, önerilerinizi düzenleyebilir, silebilir veya devre dışı edebilir ve önerilerinizi etkinleştirebilirsiniz. 

1. **Portalda**Azure **Danışmanı'nı**seçin.

    ![Azure Danışman Banner](./media/advisor-alerts/create1.png)

2. Sol menünün **İzleme** bölümünde **Uyarılar'ı**seçin.

    ![Azure Danışman Banner](./media/advisor-alerts/create2.png)

3. Bir uyarıyı yeniden yapmak için, uyarıyı açmak ve yeniden yapmak istediğiniz alanları yeniden yapmak için Uyarı adını tıklatın.

4. Bir uyarıyı silmek, etkinleştirmek veya devre dışı katmak için satırın sonundaki elipsi tıklatın ve ardından yapmak istediğiniz eylemi seçin.
 

