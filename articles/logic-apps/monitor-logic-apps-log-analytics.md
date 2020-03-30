---
title: Azure Monitor günlüklerini kullanarak mantık uygulamalarını izleme
description: Azure Monitor günlüklerini ayarlayarak ve Azure Logic Apps için tanılama verileri toplayarak mantık uygulamalarını sorun giderme
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 3e41f92f9e41f7a05102e8c0e1c2edb81fa50bf3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270244"
---
# <a name="set-up-azure-monitor-logs-and-collect-diagnostics-data-for-azure-logic-apps"></a>Azure Monitor günlüklerini ayarlayın ve Azure Logic Apps için tanılama verilerini toplayın

Çalışma sırasında mantık uygulamalarınız hakkında daha zengin hata ayıklama bilgileri elde etmek için, tetikleme olayları, etkinlik çalıştırma ve eylem olayları gibi çalışma zamanı verileri ve olayları hakkındaki bilgileri bir [Log Analytics çalışma alanında](../azure-monitor/platform/resource-logs-collect-workspace.md)kaydetmek ve depolamak için Azure Monitor [günlüklerini](../azure-monitor/platform/data-platform-logs.md) ayarlayabilir ve kullanabilirsiniz. [Azure Monitör,](../azure-monitor/overview.md) kullanılabilirliklerini ve performanslarını daha kolay koruyabilmeniz için bulut ve şirket içi ortamlarınızı izlemenize yardımcı olur. Azure Monitor günlüklerini kullanarak, bu bilgileri toplamanıza ve gözden geçirmenize yardımcı olacak [günlük sorguları](../azure-monitor/log-query/log-query-overview.md) oluşturabilirsiniz. Bu tanılama verilerini Azure Depolama ve Azure Etkinlik Hub'ları gibi diğer Azure hizmetleriyle de [kullanabilirsiniz.](#extend-data)

Mantık uygulamanız için günlük oluşturmayı ayarlamak için, [mantık uygulamanızı oluştururken Log Analytics'i etkinleştirebilir](#logging-for-new-logic-apps)veya mevcut mantık uygulamaları için Log Analytics çalışma alanınızda [Logic Apps Management çözümlerini yükleyebilirsiniz.](#install-management-solution) Bu çözüm, mantık uygulamanız için toplu bilgiler sağlar ve durum, yürütme süresi, yeniden gönderme durumu ve korelasyon disleri gibi belirli ayrıntıları içerir. Ardından, bu bilgiler için günlüğe kaydetme ve sorgu oluşturmayı etkinleştirmek için [Azure Monitor günlüklerini ayarlayın.](#set-up-resource-logs)

Bu makalede, mantıksal uygulamalar oluştururken Log Analytics'in nasıl etkinleştirilen, Logic Apps Yönetimi çözümünü nasıl yükleyip kurabileceğiniz ve Azure Monitor günlükleri için sorguların nasıl ayarlanıp oluşturulabileceğiniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce bir [Log Analytics çalışma alanına](../azure-monitor/platform/resource-logs-collect-workspace.md)ihtiyacınız vardır. Çalışma alanınız yoksa, [Log Analytics çalışma alanı oluşturmayı](../azure-monitor/learn/quick-create-workspace.md)öğrenin.

<a name="logging-for-new-logic-apps"></a>

## <a name="enable-log-analytics-for-new-logic-apps"></a>Yeni mantık uygulamaları için Log Analytics'i etkinleştirme

Mantık uygulamanızı oluştururken Log Analytics'i açabilirsiniz.

1. Mantıksal uygulamanızı oluşturmak için bilgileri sağladığınız bölmedeki [Azure portalında](https://portal.azure.com)aşağıdaki adımları izleyin:

   1. **Log Analytics**altında, **Aç'ı**seçin.

   1. Log **Analytics çalışma alanı** listesinden, mantık uygulamanız çalışmalarından verileri göndermek istediğiniz çalışma alanını seçin.

      ![Mantıksal uygulama bilgilerini sağlama](./media/monitor-logic-apps-log-analytics/create-logic-app-details.png)

      Bu adımı tamamladıktan sonra Azure, artık Log Analytics çalışma alanınızınizle ilişkili olan mantık uygulamanızı oluşturur. Ayrıca, bu adım, çalışma alanınızda Mantık Uygulamaları Yönetimi çözümlerini otomatik olarak yükler.

1. İşiniz bittiğinde **Oluştur**’u seçin.

1. Mantık uygulamanızı çalıştırdıktan sonra, mantık uygulamanızın çalışanlarını görüntülemek için [şu adımlarla devam edin.](#view-logic-app-runs)

<a name="install-management-solution"></a>

## <a name="install-logic-apps-management-solution"></a>Logic Apps Yönetimi çözümlerini yükleyin

Mantık uygulamanızı oluşturduğunuzda Log Analytics'i açtıysanız, bu adımı atlayın. Log Analytics çalışma alanınızda Zaten Logic Apps Management çözümü yüklü.

1. Azure [portalının](https://portal.azure.com)arama kutusuna `log analytics workspaces`girin ve ardından **Günlük Analizi çalışma alanlarını**seçin.

   !["Günlük Analitiği çalışma alanlarını" seçin](./media/monitor-logic-apps-log-analytics/find-select-log-analytics-workspaces.png)

1. **Günlük Analizi çalışma alanları**altında çalışma alanınızı seçin.

   ![Günlük Analizi çalışma alanınızı seçin](./media/monitor-logic-apps-log-analytics/select-log-analytics-workspace.png)

1. Genel **Bakış** bölmesinde, **Log Analytics** > ile başla nın altında**izleme çözümlerini yapılandırın,** **Çözümleri Görüntüle'yi**seçin.

   ![Genel bakış bölmede "Çözümleri görüntüle" seçeneğini belirleyin](./media/monitor-logic-apps-log-analytics/log-analytics-workspace.png)

1. **Genel Bakış**altında **Ekle'yi**seçin.

   ![Genel bakış bölmesine yeni çözüm ekleyin](./media/monitor-logic-apps-log-analytics/add-logic-apps-management-solution.png)

1. **Market** açıldıktan sonra, arama kutusuna Girin `logic apps management`ve Mantık Uygulamaları **Yönetimi'ni**seçin.

   ![Market'ten "Logic Apps Management" seçeneğini belirleyin](./media/monitor-logic-apps-log-analytics/select-logic-apps-management.png)

1. Çözüm açıklama bölmesine, **Oluştur'u**seçin.

   !["Logic Apps Management" çözümünü eklemek için "Oluştur" seçeneğini belirleyin](./media/monitor-logic-apps-log-analytics/create-logic-apps-management-solution.png)

1. Çözümü yüklemek istediğiniz Log Analytics çalışma alanını gözden geçirin ve onaylayın ve yeniden **Oluştur'u** seçin.

   !["Logic Apps Management" için "Oluştur"u seçin](./media/monitor-logic-apps-log-analytics/confirm-log-analytics-workspace.png)

   Azure, çözümü Günlük Analizi çalışma alanınızı içeren Azure kaynak grubuna dağıttıktan sonra, çözüm çalışma alanınızın özet bölmesinde görünür.

   ![Çalışma alanı özet bölmesi](./media/monitor-logic-apps-log-analytics/workspace-summary-pane-logic-apps-management.png)

<a name="set-up-resource-logs"></a>

## <a name="set-up-azure-monitor-logs"></a>Azure Monitör günlüklerini ayarlama

[Azure Monitor günlüklerinde](../azure-monitor/platform/data-platform-logs.md)çalışma zamanı olayları ve verilerle ilgili bilgileri depoladığınızda, bu bilgileri bulmanıza ve gözden geçirmenize yardımcı olacak [günlük sorguları](../azure-monitor/log-query/log-query-overview.md) oluşturabilirsiniz.

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı bulun ve seçin.

1. Mantık uygulaması menüsünde, **İzleme**altında, **Tanılama ayarlarını** > seçin**Tanılama ayarı ekleyin.**

   !["İzleme" altında "Tanılama ayarları" > "Tanılama ayarı ekle" seçeneğini belirleyin](./media/monitor-logic-apps-log-analytics/logic-app-diagnostics.png)

1. Ayarı oluşturmak için aşağıdaki adımları izleyin:

   1. Ayar için bir ad sağlayın.

   1. **Günlük Analitiğine Gönder'i**seçin.

   1. **Abonelik**için, Günlük Analizi çalışma alanınızla ilişkili Azure aboneliğini seçin.

   1. **Günlük Analizi Çalışma Alanı**için, kullanmak istediğiniz çalışma alanını seçin.

   1. **Günlük**altında, kaydetmek istediğiniz etkinlik kategorisini belirten **WorkflowRuntime** kategorisini seçin.

   1. Tüm ölçümleri seçmek için, **metrik**altında, **AllMetrics'i**seçin.

   1. İşiniz bittiğinde **Kaydet**'i seçin.

   Örnek:

   ![Günlük günlüğe kaydetme için Log Analytics çalışma alanını ve verilerini seçin](./media/monitor-logic-apps-log-analytics/send-diagnostics-data-log-analytics-workspace.png)

<a name="view-logic-app-runs"></a>

## <a name="view-logic-app-runs-status"></a>Görünüm mantık uygulaması durumu çalışır

Mantık uygulamanız çalıştırdıktan sonra, Log Analytics çalışma alanınızda bu çalıştırmalarla ilgili verileri görüntüleyebilirsiniz.

1. Azure [portalında](https://portal.azure.com)Log Analytics çalışma alanınızı bulun ve açın.

1. Çalışma alanınızın menüsünde, **Çalışma Alanı özeti** > **Logic Apps Management'ı**seçin.

   ![Mantık uygulaması çalıştırma durumu ve sayısı](./media/monitor-logic-apps-log-analytics/logic-app-runs-summary.png)

   > [!NOTE]
   > Logic Apps Management döşemesi bir çalıştırmadan sonra sonuçları hemen göstermiyorsa, Yeniden denemeden önce **Yenile'yi** seçmeyi deneyin veya kısa bir süre bekleyin.

   Burada, mantık uygulama çalışır ada veya yürütme durumuna göre gruplandırılır. Bu sayfada ayrıca, uygulama nın çalıştırdığı mantık uygulamaları için eylemlerdeki hatalar veya tetikleyiciler hakkında ayrıntılar da yer alır.

   ![Mantık uygulamanız için durum özeti çalışır](./media/monitor-logic-apps-log-analytics/logic-app-runs-summary-details.png)

1. Belirli bir mantık uygulaması veya durumu için tüm çalıştırmaları görüntülemek için, bu mantık uygulaması veya durumu için satırı seçin.

   Aşağıda, belirli bir mantık uygulamasının tüm çalıştırmalarını gösteren bir örnek verilmiştir:

   ![Mantık uygulaması nın çalışanlarını ve durumunu görüntüleme](./media/monitor-logic-apps-log-analytics/logic-app-run-details.png)

   [İzlenen özellikleri ayarladığınız](#extend-data)eylemler için, **İzlenen Özellikler** sütununda **Görünüm'u** seçerek bu özellikleri de görüntüleyebilirsiniz. İzlenen özellikleri aramak için sütun filtresini kullanın.

   ![Bir mantık uygulaması için izlenen özellikleri görüntüleme](./media/monitor-logic-apps-log-analytics/logic-app-tracked-properties.png)

   > [!NOTE]
   > İzlenen özellikler veya tamamlanan olaylar, Log Analytics çalışma alanınızda görünmeden önce 10-15 dakikalık gecikmeler yaşayabilir.
   > Ayrıca, bu sayfadaki **Yeniden Gönder** özelliği şu anda kullanılamıyor.

1. Sonuçlarınızı filtrelemek için hem istemci hem de sunucu tarafı filtreleme gerçekleştirebilirsiniz.

   * **İstemci tarafı filtresi**: Her sütun için, örneğin istediğiniz filtreleri seçin:

     ![Örnek sütun filtreleri](./media/monitor-logic-apps-log-analytics/filters.png)

   * **Sunucu tarafı filtresi**: Belirli bir zaman penceresini seçmek veya görünen çalıştırma sayısını sınırlamak için sayfanın üst kısmındaki kapsam denetimini kullanın. Varsayılan olarak, aynı anda yalnızca 1.000 kayıt görüntülenir.

     ![Zaman penceresini değiştirme](./media/monitor-logic-apps-log-analytics/change-interval.png)

1. Belirli bir çalıştırma için tüm eylemleri ve ayrıntılarını görüntülemek için, bir mantık uygulaması çalışması için satırı seçin.

   Aşağıda, belirli bir mantık uygulaması nın çalıştırılışı için tüm eylemleri ve tetikleyicileri gösteren bir örnek verilmiştir:

   ![Bir mantık uygulaması çalışması için eylemleri görüntüleme](./media/monitor-logic-apps-log-analytics/logic-app-action-details.png)

<!-------------
   * **Resubmit**: You can resubmit one or more logic apps runs that failed, succeeded, or are still running. Select the check boxes for the runs that you want to resubmit, and then select **Resubmit**.

     ![Resubmit logic app runs](./media/monitor-logic-apps-log-analytics/logic-app-resubmit.png)
--------------->

<a name="extend-data"></a>

## <a name="send-diagnostic-data-to-azure-storage-and-azure-event-hubs"></a>Tanı verilerini Azure Depolama ve Azure Etkinlik Hub'larına gönderme

Azure Monitor günlüklerinin yanı sıra, mantık uygulamanızın tanılama verilerini diğer Azure hizmetleriyle nasıl kullandığınızı genişletebilirsiniz, örneğin:

* [Azure kaynak günlüğünü depolama hesabına arşivleme](../azure-monitor/platform/resource-logs-collect-storage.md)
* [Azure platform günlüklerini Azure Etkinlik Hub'larına aktarın](../azure-monitor/platform/resource-logs-stream-event-hubs.md)

Daha sonra [Azure Akış Analizi](../stream-analytics/stream-analytics-introduction.md) ve Power [BI](../azure-monitor/platform/powerbi.md)gibi diğer hizmetlerden telemetri ve analiz kullanarak gerçek zamanlı izleme alabilirsiniz. Örnek:

* [Olay Merkezlerinden Akış Analizi'ne veri akışı](../stream-analytics/stream-analytics-define-inputs.md)
* [Akış Analizi ile akış verilerini analiz edin ve Power BI'de gerçek zamanlı analiz panosu oluşturun](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Tanılama verilerini göndermek istediğiniz konumlara göre, önce [bir Azure depolama hesabı oluşturduğunuzdan](../storage/common/storage-create-storage-account.md) veya bir Azure etkinlik hub'ı [oluşturduğunuzdan](../event-hubs/event-hubs-create.md)emin olun. Ardından, bu verileri göndermek istediğiniz hedefleri seçebilirsiniz. Bekletme süreleri yalnızca bir depolama hesabı kullandığınızda geçerlidir.

![Azure depolama hesabına veya etkinlik merkezine veri gönderme](./media/monitor-logic-apps-log-analytics/diagnostics-storage-event-hub-log-analytics.png)

<a name="diagnostic-event-properties"></a>

## <a name="azure-monitor-diagnostics-events"></a>Azure Tanılama olaylarını izleyin

Her tanılama olayının mantık uygulamanız ve bu olay, örneğin durum, başlangıç saati, bitiş saati ve benzeri ayrıntılar vardır. İzleme, izleme ve günlüğe kaydetmeyi programlı bir şekilde ayarlamak için bu bilgileri [Azure Logic Apps için REST API](https://docs.microsoft.com/rest/api/logic) ve Azure Monitor için REST [API](../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows)ile kullanabilirsiniz. Ayrıca, `clientTrackingId` `trackedProperties` 

* `clientTrackingId`: Sağlanmazsa, Azure bu kimliği otomatik olarak oluşturur ve mantık uygulamasından çağrılan iç içe iş akışları da dahil olmak üzere bir mantık uygulaması çalışmasındaki olayları ilişkilendirer. Tetikleyici isteğinde özel kimlik değeriniz olan `x-ms-client-tracking-id` bir üstbilgi geçirerek bu kimliği tetikleyicide el ile belirtebilirsiniz. İstek tetikleyicisi, HTTP tetikleyicisi veya webhook tetikleyicisi kullanabilirsiniz.

* `trackedProperties`: Tanılama verilerindeki girişleri veya çıktıları izlemek `trackedProperties` için, Logic App Designer'ı kullanarak veya doğrudan mantık uygulamanızın JSON tanımına bir bölüm ekleyebilirsiniz. İzlenen özellikler yalnızca tek bir eylemin giriş ve çıktılarını izleyebilir, `correlation` ancak bir çalıştırmadaki eylemler arasında ilişkilendirmek için olayların özelliklerini kullanabilirsiniz. Birden fazla özelliği, bir veya daha fazla `trackedProperties` özelliği izlemek için, bölümü ve istediğiniz özellikleri eylem tanımına ekleyin.

  Aşağıda, **Initialize değişken** eylem tanımının, girişin bir kayıt değil, eylem girişinden izlenen özellikleri nasıl içerdiğini gösteren bir örnek verilmiştir.

  ``` json
  {
     "Initialize_variable": {
        "type": "InitializeVariable",
        "inputs": {
           "variables": [
              {
                 "name": "ConnectorName", 
                 "type": "String", 
                 "value": "SFTP-SSH" 
              }
           ]
        },
        "runAfter": {},
        "trackedProperties": { 
           "myTrackedPropertyName": "@action().inputs.variables[0].value"
        }
     }
  }
  ```

  Bu örnekte, izlenen birden çok özellik göster:

  ``` json
  "HTTP": {
     "type": "Http",
     "inputs": {
        "body": "@triggerBody()",
        "headers": {
           "Content-Type": "application/json"
        },
        "method": "POST",
        "uri": "http://store.fabrikam.com",
     },
     "runAfter": {},
     "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
     }
  }
  ```

Bu örnek, `ActionCompleted` olayın nasıl `clientTrackingId` `trackedProperties` içerdiğini ve özniteliklerini gösterir:

```json
{
   "time": "2016-07-09T17:09:54.4773148Z",
   "workflowId": "/subscriptions/XXXXXXXXXXXXXXX/resourceGroups/MyResourceGroup/providers/Microsoft.Logic/workflows/MyLogicApp",
   "resourceId": "/subscriptions/<subscription-ID>/resourceGroups/MyResourceGroup/providers/Microsoft.Logic/workflows/MyLogicApp/runs/<run-ID>/actions/Http",
   "category": "WorkflowRuntime",
   "level": "Information",
   "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
   "properties": {
      "$schema": "2016-06-01",
      "startTime": "2016-07-09T17:09:53.4336305Z",
      "endTime": "2016-07-09T17:09:53.5430281Z",
      "status": "Succeeded",
      "code": "OK",
      "resource": {
         "subscriptionId": "<subscription-ID>",
         "resourceGroupName": "MyResourceGroup",
         "workflowId": "<logic-app-workflow-ID>",
         "workflowName": "MyLogicApp",
         "runId": "08587361146922712057",
         "location": "westus",
         "actionName": "Http"
      },
      "correlation": {
         "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
         "clientTrackingId": "<my-custom-tracking-ID>"
      },
      "trackedProperties": {
         "myTrackedPropertyName": "<value>"
      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [İzleyici ve izleme sorguları oluşturma](../logic-apps/create-monitoring-tracking-queries.md)
* [Azure İzleyici günlükleri ile B2B iletilerini izleme](../logic-apps/monitor-b2b-messages-log-analytics.md)
