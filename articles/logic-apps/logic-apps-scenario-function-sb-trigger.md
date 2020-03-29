---
title: Azure İşlevleri ile mantıksal uygulamaları çağırma
description: Azure Hizmet Veri Servisi'ni dinleyerek mantık uygulamalarını arayan veya tetikleyen Azure işlevleri oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: jehollan, klam, logicappspm
ms.topic: article
ms.date: 11/08/2019
ms.openlocfilehash: afd2735bae2a79ad942c347219019ef200b61070
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75428718"
---
# <a name="call-or-trigger-logic-apps-by-using-azure-functions-and-azure-service-bus"></a>Azure İşlevleri ve Azure Hizmet Veri Servisi'ni kullanarak mantık uygulamalarını arayın veya tetikleyin

Uzun süreli bir dinleyiciyi veya görevi dağıtmanız gerektiğinde bir mantık uygulamasını tetiklemek için [Azure İşlevlerini](../azure-functions/functions-overview.md) kullanabilirsiniz. Örneğin, Bir [Azure Hizmet Veri Yolundan](../service-bus-messaging/service-bus-messaging-overview.md) sırayı dinleyen ve anında bir mantık uygulamasını itme tetikleyicisi olarak ateşleyen bir Azure işlevi oluşturabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Azure Hizmet Veri Servisi ad alanı. Ad alanınız yoksa, önce [ad alanınızı oluşturun.](../service-bus-messaging/service-bus-create-namespace-portal.md)

* Azure işlevleri için bir kapsayıcı olan bir Azure işlev uygulaması. Bir işlev uygulamanız yoksa, [önce işlev uygulamanızı oluşturun](../azure-functions/functions-create-first-azure-function.md)ve çalışma zamanı yığını olarak .NET'i seçtiğinizden emin olun.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

## <a name="create-logic-app"></a>Mantıksal uygulama oluşturma

Bu senaryo için, tetiklemek istediğiniz her mantık uygulamasını çalıştıran bir işleviniz vardır. İlk olarak, http istek tetikleyicisi ile başlayan bir mantık uygulaması oluşturun. Bir sıra iletisi alındığı zaman işlev bu bitiş noktasını çağırır.

1. [Azure portalında](https://portal.azure.com)oturum açın ve boş mantık uygulaması oluşturun.

   Mantık uygulamalarında yeniyseniz [Quickstart'ı inceleyin: İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

1. Arama kutusuna `http request` yazın. Tetikleyiciler **listesinden, BIR HTTP isteği ne zaman tetikleyici alınır'ı** seçin.

   ![Tetikleyiciyi seçin](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

   İstek tetikleyicisi ile isteğe bağlı olarak sıra iletisiyle kullanmak üzere bir JSON şeması girebilirsiniz. JSON şemaları, Mantık Uygulama Tasarımcısı'nın giriş verilerinin yapısını anlamasına ve çıktıları iş akışınızda kullanmanıza yardımcı olur.

1. Şemayı belirtmek için, örneğin İstek Gövdesi **JSON Schema** kutusuna şemayı girin:

   ![JSON şema belirtin](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger-schema.png)

   Şema nız yoksa, ancak JSON formatında örnek bir yükünüz varsa, bu yükten bir şema oluşturabilirsiniz.

   1. İstek tetikleyicisinde **şema oluşturmak için örnek yükü kullan'ı**seçin.

   1. **Örnek bir JSON yükünü girin veya yapıştırın**altında, örnek yükünüzü girin ve ardından **Bitti'yi**seçin.

      ![Örnek taşıma yükünü girin](./media/logic-apps-scenario-function-sb-trigger/enter-sample-payload.png)

   Bu örnek yük tetikleyicide görünen bu şema oluşturur:

   ```json
   {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "number": {
                  "type": "integer"
               },
               "street": {
                  "type": "string"
               },
               "city": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "integer"
               },
               "country": {
                  "type": "string"
               }
            }
         }
      }
   }
   ```

1. Sıra iletisini aldıktan sonra çalıştırmak istediğiniz diğer eylemleri ekleyin.

   Örneğin, Office 365 Outlook bağlayıcısı ile bir e-posta gönderebilirsiniz.

1. Bu mantık uygulamasında tetikleyici için geri arama URL'sini oluşturan mantık uygulamanızı kaydedin. Daha sonra, Bu geri arama URL'sini Azure Hizmet Veri Hizmeti Sırası tetikleyicisi için kodda kullanırsınız.

   Geri arama URL'si **HTTP POST URL** özelliğinde görünür.

   ![Tetikleyici için oluşturulan geri arama URL'si](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Azure işlevi oluşturma

Ardından, tetikleyici görevi gören ve sırayı dinleyen işlevi oluşturun.

1. Azure portalında, zaten açık değilse işlev uygulamanızı açın ve genişletin. 

1. İşlev uygulama adınız **altında, İşlevler'i**genişletin. **Fonksiyonlar** bölmesine **Yeni işlevi**seçin.

   !["İşlevler"i genişletin ve "Yeni işlev" seçeneğini belirleyin](./media/logic-apps-scenario-function-sb-trigger/add-new-function-to-function-app.png)

1. Çalışma zamanı yığını olarak .NET'i seçtiğiniz yeni bir işlev uygulaması oluşturup oluşturmadığınıza veya varolan bir işlev uygulamasını kullanıp kullanmadığınıza bağlı olarak bu şablonu seçin.

   * Yeni işlev uygulamaları için şu şablonu seçin: **Hizmet Veri Servisi Sırası tetikleyicisi**

     ![Yeni işlev uygulaması için şablon u seçin](./media/logic-apps-scenario-function-sb-trigger/current-add-queue-trigger-template.png)

   * Varolan bir işlev uygulaması için şu şablonu seçin: **Service Bus Queue tetikleyicisi - C#**

     ![Varolan işlev uygulaması için şablonu seçin](./media/logic-apps-scenario-function-sb-trigger/legacy-add-queue-trigger-template.png)

1. Azure **Hizmet Veri Servisi Sıra tetikleyici** bölmesinde, tetikleyiciniz için bir ad sağlayın ve Kuyruk için Azure Hizmet Veri Servisi SDK `OnMessageReceive()` dinleyicisini kullanan Servis Veri Servisi **bağlantısını** ayarlayın ve **Oluştur'u**seçin.

1. Kuyruk iletisini tetikleyici olarak kullanarak daha önce oluşturulmuş mantık uygulaması bitiş noktasını çağırmak için temel bir işlev yazın. İşlevinizi yazmadan önce şu hususları gözden geçirin:

   * Bu örnek `application/json` ileti içeriği türünü kullanır, ancak bu türü gerektiği gibi değiştirebilirsiniz.
   
   * Olası aynı anda çalışan işlevler, yüksek hacimler veya ağır yükler nedeniyle, `using` [httpclient sınıfını](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) deyimle anında kullanmaktan ve istek başına doğrudan HTTPClient örnekleri oluşturmaktan kaçının. Daha fazla bilgi için esnek [HTTP isteklerini uygulamak için HttpClientFactory'yi kullanın' a](https://docs.microsoft.com/dotnet/architecture/microservices/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests#issues-with-the-original-httpclient-class-available-in-net-core)bakın.
   
   * Mümkünse, HTTP istemcileri örneğini yeniden kullanın. Daha fazla bilgi için Azure [İşlevlerinde Bağlantıları Yönet'e](../azure-functions/manage-connections.md)bakın.

   Bu örnek, [ `Task.Run` yöntemi](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run) [eşzamanlı](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async) modda kullanır. Daha fazla bilgi için, [async ile Asynchronous programlama](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/)bakın ve bekliyor.

   ```csharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   // Can also fetch from App Settings or environment variable
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/workflows/<remaining-callback-URL>";

   // Reuse the instance of HTTP clients if possible: https://docs.microsoft.com/azure/azure-functions/manage-connections
   private static HttpClient httpClient = new HttpClient();

   public static async Task Run(string myQueueItem, TraceWriter log) 
   {
      log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
      var response = await httpClient.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")); 
   }
   ```

1. İşlevi sınamak için, [Hizmet Veri Gönderi Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer)gibi bir araç kullanarak bir sıra iletisi ekleyin.

   Mantık uygulaması, işlev iletiyi aldıktan hemen sonra tetikler.

## <a name="next-steps"></a>Sonraki adımlar

* [HTTP uç noktalarını kullanarak iş akışlarını arama, tetikleme veya yuvalama](../logic-apps/logic-apps-http-endpoint.md)
