---
title: Azure Işlevleri ile mantıksal uygulamaları çağırma-Azure Logic Apps
description: Azure Service Bus dinleyerek Logic Apps 'i çağıran veya tetikleyen Azure işlevleri oluşturun
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jehollan, klam, LADocs
ms.topic: article
ms.date: 06/04/2019
ms.openlocfilehash: 2ab6ace7c30c3dd385928b6b0ae8000485d5f495
ms.sourcegitcommit: d37991ce965b3ee3c4c7f685871f8bae5b56adfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72680153"
---
# <a name="call-or-trigger-logic-apps-by-using-azure-functions-and-azure-service-bus"></a>Azure Işlevleri 'ni ve Azure Service Bus kullanarak mantıksal uygulamaları çağırın veya tetikleyin

Uzun süre çalışan bir dinleyici veya görev dağıtmanız gerektiğinde, bir mantıksal uygulamayı tetiklemek için [Azure işlevleri](../azure-functions/functions-overview.md) 'ni kullanabilirsiniz. Örneğin, bir [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) kuyruğu üzerinde dinleme yapan bir Azure işlevi oluşturabilir ve bir mantıksal uygulamayı anında iletme tetikleyicisi olarak tetikleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bir Azure Service Bus ad alanı. Bir ad alanınız yoksa, [önce ad alanınızı oluşturun](../service-bus-messaging/service-bus-create-namespace-portal.md).

* Azure işlevleri için kapsayıcı olan bir Azure işlev uygulaması. Bir işlev uygulamanız yoksa, [önce işlev uygulamanızı oluşturun](../azure-functions/functions-create-first-azure-function.md)ve çalışma zamanı yığını olarak .net ' i seçtiğinizden emin olun.

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

## <a name="create-logic-app"></a>Mantıksal uygulama oluşturma

Bu senaryo için, tetiklemek istediğiniz her mantıksal uygulamayı çalıştıran bir işleviniz vardır. İlk olarak, bir HTTP istek tetikleyicisi ile başlayan bir mantıksal uygulama oluşturun. İşlevi, her bir kuyruk iletisi alındığında bu uç noktayı çağırır.  

1. [Azure Portal](https://portal.azure.com)oturum açın ve boş mantıksal uygulama oluşturun.

   Logic Apps 'e yeni başladıysanız [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)konusunu inceleyin.

1. Arama kutusuna "http isteği" yazın. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **BIR http isteği alındığında**

   ![Tetikleyiciyi seçin](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

   Istek tetikleyicisi ile isteğe bağlı olarak, kuyruk iletisiyle birlikte kullanılacak bir JSON şeması girebilirsiniz. JSON şemaları, Logic App Designer 'ın giriş verilerinin yapısını anlamasına yardımcı olur ve iş akışınızda, çıktıları sizin için daha kolay hale getirir.

1. Bir şema belirtmek için, şemayı **Istek GÖVDESI JSON şemasına** girin, örneğin:

   ![JSON şeması belirt](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger-schema.png)

   Bir şemanız yoksa ancak JSON biçiminde bir örnek yüküyle karşılaşırsanız, bu yükün bir şemasını oluşturabilirsiniz.

   1. Istek tetikleyicisinde, **şema oluşturmak için örnek yük kullan**' ı seçin.

   1. **Örnek BIR JSON yükü girin veya yapıştırın**bölümüne örnek yükünüzü girip **bitti**' yi seçin.

      ![Örnek yük girin](./media/logic-apps-scenario-function-sb-trigger/enter-sample-payload.png)

   Bu örnek yük, tetikleyicide görüntülenen bu şemayı oluşturur:

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

   Örneğin, Office 365 Outlook Bağlayıcısı ile bir e-posta gönderebilirsiniz.

1. Bu mantıksal uygulamadaki tetikleyici için geri çağırma URL 'sini üreten mantıksal uygulamanızı kaydedin. Daha sonra, Azure Service Bus kuyruğu tetikleyicisinin kodunda bu geri çağırma URL 'sini kullanırsınız.

   Geri çağırma URL 'si **http post URL 'si** özelliğinde görüntülenir.

   ![Tetikleyici için geri çağırma URL 'SI oluşturuldu](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Azure işlevi oluşturma

Sonra tetikleyici olarak davranan ve kuyruğu dinleyen işlevi oluşturun.

1. Azure portal, zaten açık değilse, işlev uygulamanızı açın ve genişletin. 

1. İşlev uygulamanızın adı altında **işlevler**' i genişletin. **İşlevler** bölmesinde **yeni işlev**' ı seçin.

   !["Işlevler" i genişletin ve "yeni işlev" i seçin](./media/logic-apps-scenario-function-sb-trigger/create-new-function.png)

1. Çalışma zamanı yığını olarak .NET seçtiğiniz yeni bir işlev uygulaması oluşturup oluşturamayacağını veya var olan bir işlev uygulamasını kullandığınızı temel alarak bu şablonu seçin.

   * Yeni işlev uygulamaları için şu şablonu seçin: **Service Bus kuyruğu tetikleyicisi**

     ![Yeni işlev uygulaması için şablon seçin](./media/logic-apps-scenario-function-sb-trigger/current-add-queue-trigger-template.png)

   * Mevcut bir işlev uygulaması için şu şablonu seçin: **Service Bus kuyruğu tetikleyicisi- C#**

     ![Mevcut işlev uygulaması için şablon seçin](./media/logic-apps-scenario-function-sb-trigger/legacy-add-queue-trigger-template.png)

1. **Azure Service Bus kuyruğu tetikleyicisi** bölmesinde, Tetikleyiciniz için bir ad sağlayın ve Azure Service Bus SDK `OnMessageReceive()` dinleyicisi kullanan ve **Oluştur**' u seçerek **Service Bus bağlantısını** ayarlayın.

1. Kuyruk iletisini tetikleyici olarak kullanarak, önceden oluşturulmuş mantıksal uygulama uç noktasını çağırmak için temel bir işlev yazın. Bu örnek `application/json` ileti içerik türünü kullanır, ancak bu türü gerektiği gibi değiştirebilirsiniz. Mümkünse, HTTP istemcilerinin örneğini yeniden kullanın. Daha fazla bilgi için bkz. [Azure işlevlerinde bağlantıları yönetme](../azure-functions/manage-connections.md).

   ```CSharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   // Callback URL for previously created Request trigger
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/workflows/<remaining-callback-URL>";

   // Reuse the instance of HTTP clients if possible
   private static HttpClient httpClient = new HttpClient();

   public static void Run(string myQueueItem, ILogger log)
   {
       log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

       var response = httpClient.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
   }
   ```

1. İşlevi test etmek için, [Service Bus Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer)gibi bir araç kullanarak bir kuyruk iletisi ekleyin.

   Mantıksal uygulama, işlev iletiyi aldıktan hemen sonra tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

[HTTP uç noktalarını kullanarak iş akışlarını çağırma, tetikleme veya iç içe geçme](../logic-apps/logic-apps-http-endpoint.md)