---
title: Azure Monitor uyarılarıyla karmaşık eylemleri tetikleme
description: Azure Monitor uyarılarını işlemek için nasıl bir mantık uygulaması eylemi oluşturabilirsiniz öğrenin.
author: dkamstra
ms.author: dukek
ms.topic: conceptual
ms.date: 07/18/2018
ms.subservice: alerts
ms.openlocfilehash: 655a3acc44a1418778b37fbef85e5df75d042317
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206245"
---
# <a name="how-to-trigger-complex-actions-with-azure-monitor-alerts"></a>Azure Monitor uyarılarıyla karmaşık eylemleri tetikleme

Bu makalede, bir uyarı çıktığında Microsoft Teams'te bir konuşma oluşturmak için bir mantık uygulamasının nasıl ayarlanıp tetiklenileceğiniz gösterilmektedir.

## <a name="overview"></a>Genel Bakış

Bir Azure Monitörü uyarısı tetiklendiğinde, bir [eylem grubunu](../../azure-monitor/platform/action-groups.md)çağırır. Eylem grupları, bir uyarı hakkında başkalarını bilgilendirmek ve aynı zamanda düzeltmek için bir veya daha fazla eylemi tetiklemenize olanak sağlar.

Genel süreç:

-   İlgili uyarı türü için mantık uygulamasını oluşturun.

-   İlgili uyarı türü için örnek bir yükü mantık uygulamasına aktarın.

-   Mantık uygulaması davranışını tanımlayın.

-   Mantık uygulamasının HTTP bitiş noktasını bir Azure eylem grubuna kopyalayın.

Mantık uygulamasının farklı bir eylem gerçekleştirmesini istiyorsanız, işlem benzer.

## <a name="create-an-activity-log-alert-administrative"></a>Etkinlik günlüğü uyarısı oluşturma: Yönetim

1.  Azure portalında, sol üst köşede **kaynak oluştur'u** seçin.

2.  **Mantık Uygulamasını**arayın ve seçin, ardından **Oluştur'u**seçin.

3.  Mantık uygulamanıza bir **Ad**verin, bir **Kaynak grubu**seçin ve böyle devam edin.

    ![Mantıksal uygulama oluşturma](media/action-groups-logic-app/create-logic-app-dialog.png "Mantıksal uygulama oluşturma")

4.  Mantık uygulamasını oluşturmak için **Oluştur'u** seçin. Açılır ileti, mantık uygulamasının oluşturulduğunu gösterir. **Logic Apps Tasarımcısı'nı**açmak için **Başlat Kaynağını** seçin.

5.  Tetikleyiciyi seçin: **Bir HTTP isteği aldığında.**

    ![Mantıksal uygulama tetikleyicileri](media/action-groups-logic-app/logic-app-triggers.png "Mantıksal uygulama tetikleyicileri")

6.  HTTP isteği tetikleyicisini değiştirmek için **Edit'i** seçin.

    ![HTTP isteği tetikler](media/action-groups-logic-app/http-request-trigger-shape.png "HTTP isteği tetikler")

7.  **Şema oluşturmak için örnek yük kullanma** öğesini seçin.

    ![Örnek bir yük kullanma](media/action-groups-logic-app/use-sample-payload-button.png "Örnek bir yük kullanma")

8.  Aşağıdaki örnek yükü iletişim kutusuna kopyalayıp yapıştırın:

    ```json
        {
            "schemaId": "Microsoft.Insights/activityLogs",
            "data": {
                "status": "Activated",
                "context": {
                "activityLog": {
                    "authorization": {
                    "action": "microsoft.insights/activityLogAlerts/write",
                    "scope": "/subscriptions/…"
                    },
                    "channels": "Operation",
                    "claims": "…",
                    "caller": "logicappdemo@contoso.com",
                    "correlationId": "91ad2bac-1afa-4932-a2ce-2f8efd6765a3",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-04-03T22:33:11.762469+00:00",
                    "eventDataId": "ec74c4a2-d7ae-48c3-a4d0-2684a1611ca0",
                    "level": "Informational",
                    "operationName": "microsoft.insights/activityLogAlerts/write",
                    "operationId": "61f59fc8-1442-4c74-9f5f-937392a9723c",
                    "resourceId": "/subscriptions/…",
                    "resourceGroupName": "LOGICAPP-DEMO",
                    "resourceProviderName": "microsoft.insights",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:33:36.1068742+00:00",
                    "resourceType": "microsoft.insights/activityLogAlerts"
                }
                },
                "properties": {}
            }
        }
    ```

9. **Logic App Designer,** mantık uygulamasına gönderilen isteğin **İçerik Türü** üstbilgisini **uygulama/json**olarak ayarlaması gerektiğini hatırlatmak için bir açılır pencere görüntüler. Açılır pencereyi kapatın. Azure Monitör uyarısı üstbilgi ayarlar.

    ![İçerik Türü üstbilgisini ayarlama](media/action-groups-logic-app/content-type-header.png "İçerik Türü üstbilgisini ayarlama")

10. **+** **Yeni adım'ı** seçin ve ardından **eylem ekle'yi**seçin.

    ![Eylem ekleme](media/action-groups-logic-app/add-action.png "Eylem ekleme")

11. Microsoft Teams bağlayıcısını arayın ve seçin. Microsoft **Teams - Post ileti** eylemi'ni seçin.

    ![Microsoft Teams eylemleri](media/action-groups-logic-app/microsoft-teams-actions.png "Microsoft Teams eylemleri")

12. Microsoft Teams eylemini yapılandırın. **Logic Apps Tasarımcısı,** Office 365 hesabınıza kimlik doğrulamanızı ister. İletiyi göndermek için **Takım Kimliği** ve **Kanal Kimliği'ni** seçin.

13. Dinamik içerikteki \<alanlara\> yapılan statik metin ve başvuruların bir birleşimini kullanarak iletiyi yapılandırın. Aşağıdaki metni **İleti** alanına kopyalayıp yapıştırın:

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    Ardından \<alanları\> aynı adı taşıyan dinamik içerik etiketleri ile arayın ve değiştirin.

    > [!NOTE]
    > **Durum**adlı iki dinamik alan vardır. İletiye bu alanların her ikisini de ekleyin. **ActivityLog** özellik çantasındaki alanı kullanın ve diğer alanı silin. İmlecinizi, aşağıdaki ekran görüntüsünde gösterildiği gibi tam nitelikli alan referansını görmek için **durum** alanının üzerine takın:

    ![Microsoft Teams eylemi: İleti gönderme](media/action-groups-logic-app/teams-action-post-message.png "Microsoft Teams eylemi: İleti gönderme")

14. **Logic Apps Designer'ın**en üstünde, mantık uygulamanızı kaydetmek için **Kaydet'i** seçin.

15. Varolan eylem grubunuzu açın ve mantık uygulamasına başvurmak için bir eylem ekleyin. Varolan bir eylem grubunuz yoksa, oluşturmak için [Azure portalında eylem grupları oluştur ve yönet'](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) e bakın. Değişikliklerinizi kaydetmeyi unutmayın.

    ![Eylem grubunu güncelleştirme](media/action-groups-logic-app/update-action-group.png "Eylem grubunu güncelleştirme")

Bir sonraki uyarı eylem grubunuzu aradığında, mantık uygulamanız çağrılır.

## <a name="create-a-service-health-alert"></a>Hizmet durumu uyarısı oluşturma

Azure Hizmet Durumu girişleri etkinlik günlüğünün bir parçasıdır. Uyarı oluşturma işlemi [bir etkinlik günlüğü uyarısı oluşturmaya](#create-an-activity-log-alert-administrative)benzer, ancak birkaç değişiklik ile:

- 1'den 7'ye kadar olan adımlar aynıdır.
- Adım 8 için, HTTP isteği tetikleyicisi için aşağıdaki örnek yükü kullanın:

    ```json
    {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "channels": "Admin",
                    "correlationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "description": "…",
                    "eventSource": "ServiceHealth",
                    "eventTimestamp": "2018-04-03T22:44:43.7467716+00:00",
                    "eventDataId": "9ce152f5-d435-ee31-2dce-104228486a6d",
                    "level": "Informational",
                    "operationName": "Microsoft.ServiceHealth/incident/action",
                    "operationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "properties": {
                        "title": "...",
                        "service": "...",
                        "region": "Global",
                        "communication": "...",
                        "incidentType": "Incident",
                        "trackingId": "...",
                        "impactStartTime": "2018-03-22T21:40:00.0000000Z",
                        "impactMitigationTime": "2018-03-22T21:41:00.0000000Z",
                        "impactedServices": "[{"ImpactedRegions"}]",
                        "defaultLanguageTitle": "...",
                        "defaultLanguageContent": "...",
                        "stage": "Active",
                        "communicationId": "11000001466525",
                        "version": "0.1.1"
                    },
                    "status": "Active",
                    "subscriptionId": "...",
                    "submissionTimestamp": "2018-04-03T22:44:50.8013523+00:00"
                }
            },
            "properties": {}
        }
    }
    ```

-  9 ve 10 adımları aynıdır.
-  11 ile 14 arasında adımlar için aşağıdaki işlemi kullanın:

   1. **+** **Yeni adım'ı** seçin ve ardından **koşul ekle'yi**seçin. Mantık uygulamasının yalnızca giriş verileri aşağıdaki değerlerle eşleştiğinde yürütülecek şekilde aşağıdaki koşulları ayarlayın.  Metin kutusuna sürüm değerini girerken, sayısal bir tür değil, dize olarak değerlendirildiğinden emin olmak için etrafına tırnak işaretleri ("0.1.1") koyun.  Sayfaya geri döndüğünüzde sistem tırnak tırnaklarını göstermez, ancak temel kod dize türünü korur.   
       - `schemaId == Microsoft.Insights/activityLogs`
       - `eventSource == ServiceHealth`
       - `version == "0.1.1"`

      !["Hizmet Sağlık yük durumu"](media/action-groups-logic-app/service-health-payload-condition.png "Hizmet Sağlık yük durumu")

   1. **Microsoft** Teams eylemini eklemek için etkinlik günlüğü uyarısı [oluşturma'da](#create-an-activity-log-alert-administrative) 11 ile 13 adımlarında yönergeleri izleyin.

   1. HTML ve dinamik içeriğin birleşimini kullanarak iletiyi tanımlayın. Aşağıdaki içeriği **Kopyalayıp İleti** alanına yapıştırın. Aynı `[incidentType]`ada ait dinamik içerik etiketleri ile , , `[trackingID]` `[title]`ve `[communication]` alanları değiştirin:

       ```html
       <p>
       <b>Alert Type:</b>&nbsp;<strong>[incidentType]</strong>&nbsp;
       <strong>Tracking ID:</strong>&nbsp;[trackingId]&nbsp;
       <strong>Title:</strong>&nbsp;[title]</p>
       <p>
       <a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.</a>
       </p>
       <p>[communication]</p>
       ```

       !["Hizmet Sağlık gerçek durum sonrası eylem"](media/action-groups-logic-app/service-health-true-condition-post-action.png "Hizmet Sağlık gerçek durum sonrası eylem")

   1. If **yanlış** koşul için yararlı bir ileti sağlayın:

       ```html
       <p><strong>Service Health Alert</strong></p>
       <p><b>Unrecognized alert schema</b></p>
       <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.\</a></p>
       ```

       !["Hizmet Sağlık yanlış durum sonrası eylem"](media/action-groups-logic-app/service-health-false-condition-post-action.png "Hizmet Sağlığı yanlış durum sonrası eylem")

- Adım 15 aynı. Mantık uygulamanızı kaydetmek ve eylem grubunuzu güncellemek için yönergeleri izleyin.

## <a name="create-a-metric-alert"></a>Metrik uyarı oluşturma

Metrik uyarı oluşturma işlemi, [bir etkinlik günlüğü uyarısı oluşturmaya](#create-an-activity-log-alert-administrative)benzer, ancak birkaç değişiklikle:

- 1'den 7'ye kadar olan adımlar aynıdır.
- Adım 8 için, HTTP isteği tetikleyicisi için aşağıdaki örnek yükü kullanın:

    ```json
    {
    "schemaId": "AzureMonitorMetricAlert",
    "data": {
        "version": "2.0",
        "status": "Activated",
        "context": {
        "timestamp": "2018-04-09T19:00:07.7461615Z",
        "id": "...",
        "name": "TEST-VM CPU Utilization",
        "description": "",
        "conditionType": "SingleResourceMultipleMetricCriteria",
        "condition": {
            "windowSize": "PT15M",
            "allOf": [
                {
                    "metricName": "Percentage CPU",
                    "dimensions": [
                    {
                        "name": "ResourceId",
                        "value": "d92fc5cb-06cf-4309-8c9a-538eea6a17a6"
                    }
                ],
                "operator": "GreaterThan",
                "threshold": "5",
                "timeAggregation": "PT15M",
                "metricValue": 1.0
            }
            ]
        },
        "subscriptionId": "...",
        "resourceGroupName": "TEST",
        "resourceName": "test-vm",
        "resourceType": "Microsoft.Compute/virtualMachines",
        "resourceId": "...",
        "portalLink": "..."
        },
        "properties": {}
    }
    }
    ```

- 9 ve 10 adımları aynıdır.
- 11 ile 14 arasında adımlar için aşağıdaki işlemi kullanın:

  1. **+** **Yeni adım'ı** seçin ve ardından **koşul ekle'yi**seçin. Mantık uygulamasının yalnızca giriş verileri aşağıdaki değerlerle eşleştiğinde çalıştırılabilmeleri için aşağıdaki koşulları ayarlayın. Metin kutusuna sürüm değerini girerken, yerine tırnak işaretleri ("2.0") koyarak, bunun sayısal bir tür olarak değil, bir dize olarak değerlendirilmesini sağlar.  Sayfaya geri döndüğünüzde sistem tırnak tırnaklarını göstermez, ancak temel kod dize türünü korur. 
     - `schemaId == AzureMonitorMetricAlert`
     - `version == "2.0"`
       
       !["Metrik uyarı yükü durumu"](media/action-groups-logic-app/metric-alert-payload-condition.png "Metrik uyarı yükü durumu")

  1. Eğer **gerçek** durumda, her döngü ve Microsoft Teams eylem **için** bir ekleyin. HTML ve dinamik içeriğin birleşimini kullanarak iletiyi tanımlayın.

      !["Metrik uyarı gerçek durum sonrası eylem"](media/action-groups-logic-app/metric-alert-true-condition-post-action.png "Metrik uyarı gerçek durum sonrası eylem")

  1. If **yanlış** durumunda, metrik uyarının mantık uygulamasının beklentileriyle eşleşmediğini bildirmek için bir Microsoft Teams eylemi tanımlayın. JSON yükünü de dahil et. İfadedeki dinamik `triggerBody` içeriğe nasıl `json()` başvurulmaya dikkat edin.

      !["Metrik uyarı yanlış durum sonrası eylem"](media/action-groups-logic-app/metric-alert-false-condition-post-action.png "Metrik uyarı yanlış durum sonrası eylem")

- Adım 15 aynı. Mantık uygulamanızı kaydetmek ve eylem grubunuzu güncellemek için yönergeleri izleyin.

## <a name="calling-other-applications-besides-microsoft-teams"></a>Microsoft Teams dışındaki diğer uygulamaları arama
Logic Apps, çok çeşitli uygulamalarda ve veritabanlarında eylemleri tetiklemenize olanak tanıyan bir dizi farklı bağlayıcıya sahiptir. Bolluk, SQL Server, Oracle, Salesforce, sadece bazı örneklerdir. Bağlayıcılar hakkında daha fazla bilgi için [Logic App konektörlerine](../../connectors/apis-list.md)bakın.  

## <a name="next-steps"></a>Sonraki adımlar
* Azure [etkinlik günlüğü uyarılarına genel](../../azure-monitor/platform/alerts-overview.md) bir bakış alın ve uyarıları nasıl alacağınızı öğrenin.  
* Azure Hizmet Durumu bildirimi yayınlandığında uyarıları nasıl [yapılandırıştırığınızda](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)öğrenin.
* [Eylem grupları](../../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.

