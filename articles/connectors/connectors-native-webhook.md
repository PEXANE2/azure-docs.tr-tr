---
title: Olayları bekleme ve yanıtla
description: Azure Mantık Uygulamaları'nı kullanarak hizmet bitiş noktasındaki olayları tetikleyen, duraklatan ve devam ettiren iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/06/2020
tags: connectors
ms.openlocfilehash: 0a3fb9a8a72b384d2af4af38bdc382e541ddf535
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656290"
---
# <a name="create-and-run-automated-event-based-workflows-by-using-http-webhooks-in-azure-logic-apps"></a>Azure Mantık Uygulamaları'nda HTTP webhooks'u kullanarak otomatik olay tabanlı iş akışları oluşturun ve çalıştırın

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik HTTP Webhook bağlayıcısı ile, bir HTTP veya HTTPS bitiş noktasında meydana gelen belirli olaylara göre bekleyen ve çalışan iş akışlarını mantıksal uygulamalar oluşturarak otomatikleştirebilirsiniz. Örneğin, iş akışını tetiklemeden önce belirli bir olayı bekleyerek ve belirtilen eylemleri çalıştırmadan önce, bu bitiş noktasını düzenli olarak denetlemek veya *yoklamak* yerine, bir hizmet bitiş noktasını izleyen bir mantık uygulaması oluşturabilirsiniz.

Aşağıda bazı örnek olay tabanlı iş akışları verilmiştir:

* Bir mantıksal uygulama çalışmasını tetiklemeden önce bir öğenin [Azure Etkinlik Hub'ından](https://github.com/logicappsio/EventHubAPI) gelmesini bekleyin.
* İş akışına devam etmeden önce onay bekleyin.

## <a name="how-do-webhooks-work"></a>Webhooks nasıl çalışır?

BIR HTTP webhook tetikleyicisi olay tabanlıdır ve bu da yeni öğeler için düzenli olarak denetlemeye veya yoklamaya bağlı değildir. Webhook tetikleyicisiyle başlayan bir mantık uygulamasını kaydettiğinizde veya mantık uygulamanızı devre dışı bırakılmıştan etkine değiştirdiğinizde, webhook tetikleyicisi, bu hizmete veya bitiş noktasına bir *geri arama URL'si* kaydederek belirli bir hizmete veya bitiş noktasına abone *olur.* Tetikleyici daha sonra, mantık uygulamasını çalıştırmaya başlayan URL'yi aramak için bu hizmetin veya bitiş noktasının beklemesini bekler. [İstek tetikleyicisine](connectors-native-reqres.md)benzer şekilde, belirtilen olay gerçekleştiğinde mantık uygulaması hemen devreye girer. Tetikleyiciyi kaldırıp mantık uygulamanızı kaydederseniz veya mantık uygulamanızı etkinolandan devre dışı bırakılırsa, tetikleyici hizmetten veya bitiş noktasından *aboneliğini iptal* eder.

Bir HTTP webhook eylemi de olay tabanlıdır ve bu hizmetveya bitiş noktası ile bir *geri arama URL'si* kaydederek belirli bir hizmete veya bitiş noktasına abone *olur.* Webhook eylemi mantık uygulamasının iş akışını duraklatabilir ve mantık uygulaması çalışmaya devam etmeden önce hizmetin veya bitiş noktasının URL'yi aramasını bekler. Eylem mantığı uygulaması bu gibi durumlarda hizmetten veya bitiş noktasından *aboneliğini iptal eder:*

* Webhook eylemi başarıyla bittiğinde
* Yanıt beklerken mantık uygulaması çalışması iptal edilirse
* Mantık uygulaması zaman ları dolmadan önce

Örneğin, Office 365 Outlook bağlayıcısının [**onay e-posta gönder**](connectors-create-api-office365-outlook.md) eylemi, bu deseni izleyen webhook eylemine bir örnektir. Webhook eylemini kullanarak bu deseni herhangi bir hizmete genişletebilirsiniz.

> [!NOTE]
> Logic Apps, http webhook tetikleyicisine veya eylemine geri çağrı alırken Aktarım Katmanı Güvenliği (TLS) 1.2'yi zorlar. TLS el sıkışma hataları görürseniz, TLS 1.2 kullandığınızdan emin olun. Gelen aramalar için desteklenen şifreleme paketleri şunlardır:
>
> * TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
> * TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
> * TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
> * TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
> * TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
> * TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
> * TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
> * TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

Daha fazla bilgi için şu konulara bakın:

* [HTTP Webhook tetikleme parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)
* [Webhooks ve abonelikler](../logic-apps/logic-apps-workflow-actions-triggers.md#webhooks-and-subscriptions)
* [Web hook'u destekleyen özel API'ler oluşturma](../logic-apps/logic-apps-create-api-app.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantık uygulamalarında webhook tetikleyicileri](../logic-apps/logic-apps-create-api-app.md#webhook-triggers) için webhook aboneliğini ve aboneliğini kaldıran zaten dağıtılmış bir uç nokta veya API'nin URL'si, mantık uygulamalarında veya [mantık uygulamalarındaki webhook eylemlerinde](../logic-apps/logic-apps-create-api-app.md#webhook-actions) uygun

* [Mantık uygulamaları oluşturmak için nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md)temel bilgi . Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

* Hedef bitiş noktasında belirli olayları beklemek istediğiniz mantık uygulaması. HTTP Webhook tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) HTTP Webhook eylemini kullanmak için mantık uygulamanızı istediğiniz tetikleyiciyle başlatın. Bu örnekte ilk adım olarak HTTP tetikleyicisi kullanır.

## <a name="add-an-http-webhook-trigger"></a>HTTP Webhook tetikleyicisi ekleme

Bu yerleşik tetikleyici, hedef servisteki abone bitiş noktasını çağırır ve hedef hizmete bir geri arama URL'si kaydeder. Mantık uygulamanız daha sonra hedef hizmetin `HTTP POST` geri arama URL'sine bir istek göndermesini bekler. Bu olay gerçekleştiğinde, tetikleyici iş akışı boyunca istekteki tüm verileri çalıştırAr ve aktarAr.

1. [Azure portalında](https://portal.azure.com)oturum açın. Boş mantık uygulamanızı Logic App Designer'da açın.

1. Tasarımcının arama kutusuna filtreniz olarak girin. `http webhook` **Tetikleyiciler** listesinden HTTP **Webhook** tetikleyicisini seçin.

   ![HTTP Webhook tetikleyicisi seçin](./media/connectors-native-webhook/select-http-webhook-trigger.png)

   Bu örnek, adımın `HTTP Webhook trigger` daha açıklayıcı bir ada sahip olması için tetikleyiciyi yeniden adlandırır. Ayrıca, örnek daha sonra bir HTTP Webhook eylemi ekler ve her iki ad benzersiz olmalıdır.

1. Abone olmak ve abonelikten çıkma çağrıları için kullanmak istediğiniz [HTTP Webhook tetikleyici parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger) için değerleri sağlayın.

   Bu örnekte, tetikleyici, abone ve abonelikten çıkma işlemlerini gerçekleştirirken kullanılacak yöntemleri, ÜR'leri ve ileti gövdelerini içerir.

   ![HTTP Webhook tetikleme parametrelerini girin](./media/connectors-native-webhook/http-webhook-trigger-parameters.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Abonelik - Yöntem** | Evet | Hedef bitiş noktasına abone yken kullanılacak yöntem |
   | **Abone Ol - URI** | Evet | Hedef uç noktasına abone olmak için kullanılacak URL |
   | **Abone - Gövde** | Hayır | Abone isteğine dahil edilmesi gereken tüm ileti gövdesi. Bu örnek, mantık uygulamanızın geri arama URL'sini almak için ifadeyi `@listCallbackUrl()` kullanarak, mantık uygulamanız olan aboneyi benzersiz olarak tanımlayan geri arama URL'sini içerir. |
   | **Aboneliği Iptal Et - Yöntem** | Hayır | Hedef uç noktadan aboneliği nizveederken kullanılacak yöntem |
   | **Aboneliği iptal et - URI** | Hayır | Hedef uç noktadan aboneliği nikullanmak için kullanılacak URL |
   | **Aboneliği Iptal Et - Gövde** | Hayır | Aboneliği iptal isteğine dahil edilmesi isteğe bağlı ileti gövdesi <p><p>**Not**: Bu özellik `listCallbackUrl()` işlevi kullanmayı desteklemez. Ancak, tetikleyici otomatik olarak içerir ve `x-ms-client-tracking-id` üstbilgi gönderir ve `x-ms-workflow-operation-name`, hedef hizmet benzersiz abone tanımlamak için kullanabilirsiniz. |
   ||||

1. Diğer tetikleyici özellikleri eklemek için **yeni parametre** ekle listesini açın.

   ![Daha fazla tetikleyici özelliği ekleme](./media/connectors-native-webhook/http-webhook-trigger-add-properties.png)

   Örneğin, kimlik doğrulamayı kullanmanız gerekiyorsa, Abone **Ol - Kimlik Doğrulama** ve **Aboneliği Kaldır - Kimlik Doğrulama** özelliklerini ekleyebilirsiniz. HTTP Webhook için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için [bkz.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

1. Tetikleyici ateşlendiğinde çalışan eylemlerle mantık uygulamanızın iş akışını oluşturmaya devam edin.

1. Işiniz bittiğinde, işiniz bittiğinde, mantık uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

   Mantık uygulamanızı kaydetmek, hedef servisteki abone bitiş noktasını çağırır ve geri arama URL'sini kaydeder. Mantık uygulamanız daha sonra hedef hizmetin `HTTP POST` geri arama URL'sine bir istek göndermesini bekler. Bu olay gerçekleştiğinde, tetikleyici iş akışı boyunca istekteki tüm verileri çalıştırAr ve aktarAr. Bu işlem başarıyla tamamlanırsa, tetikleyici bitiş noktasından aboneliğini iptal eder ve mantık uygulamanız kalan iş akışını sürdürür.

## <a name="add-an-http-webhook-action"></a>HTTP Webhook eylemi ekleme

Bu yerleşik eylem, hedef hizmetteki abone bitiş noktasını çağırır ve hedef hizmete bir geri arama URL'si kaydeder. Mantık uygulamanız daha sonra duraklar ve hedef `HTTP POST` hizmetin geri arama URL'sine bir istek göndermesini bekler. Bu olay gerçekleştiğinde, eylem istekteki tüm verileri iş akışına geçirir. İşlem başarıyla tamamlanırsa, eylem bitiş noktasından aboneliğini iptal eder ve mantık uygulamanız kalan iş akışını çalıştırmaya devam eder.

1. [Azure portalında](https://portal.azure.com)oturum açın. Mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte ilk adım olarak HTTP Webhook tetikleyicisi kullanır.

1. HTTP Webhook eylemini eklemek istediğiniz adımın altında **Yeni adımı**seçin.

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Tasarımcının arama kutusuna filtreniz olarak girin. `http webhook` **Eylemler** listesinden HTTP **Webhook** eylemini seçin.

   ![HTTP Webhook eylemini seçin](./media/connectors-native-webhook/select-http-webhook-action.png)

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için eylemi "HTTP Webhook eylemi" olarak yeniden adlandırır.

1. Abone olmak ve abonelikten çıkma çağrıları için kullanmak istediğiniz [HTTP Webhook tetikleyici parametrelerine](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)benzeyen HTTP Webhook eylem parametreleri için gereken değerleri sağlayın.

   Bu örnekte, eylem, abone ve abonelikten çıkma işlemlerini gerçekleştirirken kullanılacak yöntemleri, ÜR'leri ve ileti gövdelerini içerir.

   ![HTTP Webhook eylem parametrelerini girin](./media/connectors-native-webhook/http-webhook-action-parameters.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Abonelik - Yöntem** | Evet | Hedef bitiş noktasına abone yken kullanılacak yöntem |
   | **Abone Ol - URI** | Evet | Hedef uç noktasına abone olmak için kullanılacak URL |
   | **Abone - Gövde** | Hayır | Abone isteğine dahil edilmesi gereken tüm ileti gövdesi. Bu örnek, mantık uygulamanızın geri arama URL'sini almak için ifadeyi `@listCallbackUrl()` kullanarak, mantık uygulamanız olan aboneyi benzersiz olarak tanımlayan geri arama URL'sini içerir. |
   | **Aboneliği Iptal Et - Yöntem** | Hayır | Hedef uç noktadan aboneliği nizveederken kullanılacak yöntem |
   | **Aboneliği iptal et - URI** | Hayır | Hedef uç noktadan aboneliği nikullanmak için kullanılacak URL |
   | **Aboneliği Iptal Et - Gövde** | Hayır | Aboneliği iptal isteğine dahil edilmesi isteğe bağlı ileti gövdesi <p><p>**Not**: Bu özellik `listCallbackUrl()` işlevi kullanmayı desteklemez. Ancak, eylem otomatik olarak içerir ve `x-ms-client-tracking-id` üstbilgi gönderir ve `x-ms-workflow-operation-name`, hedef hizmet benzersiz abone tanımlamak için kullanabilirsiniz. |
   ||||

1. Diğer eylem özellikleri eklemek için **yeni parametre** ekle listesini açın.

   ![Daha fazla eylem özelliği ekleme](./media/connectors-native-webhook/http-webhook-action-add-properties.png)

   Örneğin, kimlik doğrulamayı kullanmanız gerekiyorsa, Abone **Ol - Kimlik Doğrulama** ve **Aboneliği Kaldır - Kimlik Doğrulama** özelliklerini ekleyebilirsiniz. HTTP Webhook için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için [bkz.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

1. Işiniz bittiğinde, mantık uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

   Şimdi, bu eylem çalıştığında, mantık uygulamanız hedef servisteki abone bitiş noktasını çağırır ve geri arama URL'sini kaydeder. Mantık uygulaması daha sonra iş akışını duraklatabilir ve `HTTP POST` hedef hizmetin geri arama URL'sine istek göndermesini bekler. Bu olay gerçekleştiğinde, eylem istekteki tüm verileri iş akışına geçirir. İşlem başarıyla tamamlanırsa, eylem bitiş noktasından aboneliğini iptal eder ve mantık uygulamanız kalan iş akışını çalıştırmaya devam eder.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Birbirine benzeyen tetikleme ve eylem parametreleri hakkında daha fazla bilgi için [HTTP Webhook parametrelerine](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)bakın.

### <a name="output-details"></a>Çıktı ayrıntıları

Bu bilgileri döndüren bir HTTP Webhook tetikleyicisinden veya eyleminden çıkan çıktılar hakkında daha fazla bilgi aşağıda verebiş tir:

| Özellik adı | Tür | Açıklama |
|---------------|------|-------------|
| Üstbilgi | object | İstekteki üstbilgi |
| body | object | JSON nesnesi | İstekten gövde içeriğine sahip nesne |
| durum kodu | int | İstekteki durum kodu |
|||

| Durum kodu | Açıklama |
|-------------|-------------|
| 200 | Tamam |
| 202 | Accepted |
| 400 | Kötü istek |
| 401 | Yetkisiz |
| 403 | Yasak |
| 404 | Bulunamadı |
| 500 | İç sunucu hatası. Bilinmeyen hata oluştu. |
|||

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
