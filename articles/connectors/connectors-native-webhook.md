---
title: Olayları bekle ve Yanıtla
description: Azure Logic Apps kullanarak bir hizmet uç noktasındaki olaylara göre tetikleme, duraklatma ve devam eden iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/06/2020
tags: connectors
ms.openlocfilehash: 1578ca030bc8bab971a44e1afcce1d1ab9e1d5e9
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78674052"
---
# <a name="create-and-run-automated-event-based-workflows-by-using-http-webhooks-in-azure-logic-apps"></a>Azure Logic Apps 'de HTTP Web kancalarını kullanarak otomatik olay tabanlı iş akışları oluşturun ve çalıştırın

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik http Web kancası Bağlayıcısı sayesinde, mantıksal uygulamalar oluşturarak http veya HTTPS uç noktasında gerçekleşen belirli olaylara dayalı olarak bekleyen ve çalışan iş akışlarını otomatikleştirebilirsiniz. Örneğin, bir hizmet uç noktasını, iş akışını tetiklemeden önce belirli bir olayı bekleyerek ve bu uç noktayı düzenli olarak denetlemek veya *yoklamak* yerine, belirtilen eylemleri çalıştırarak izleyen bir mantıksal uygulama oluşturabilirsiniz.

Örnek olay tabanlı bazı iş akışları aşağıda verilmiştir:

* Mantıksal uygulama çalıştırmayı tetiklemeden önce bir öğenin [Azure Olay Hub 'ından](https://github.com/logicappsio/EventHubAPI) gelmesini bekleyin.
* Bir iş akışına devam etmeden önce onay bekle.

## <a name="how-do-webhooks-work"></a>Web kancaları nasıl çalışır?

HTTP Web kancası tetikleyicisi, olay tabanlıdır ve yeni öğeler için düzenli olarak denetim veya yoklamaya bağlı değildir. Bir Web kancası tetikleyicisi ile başlayan bir mantıksal uygulamayı kaydettiğinizde veya mantıksal uygulamanızı devre dışı iken etkin olarak değiştirdiğinizde Web kancası tetikleyicisi, bu hizmet veya uç noktaya *geri çağırma URL 'si* kaydederek belirli bir hizmete veya uç noktaya *abone olur* . Tetikleyici daha sonra bu hizmetin veya uç noktanın mantıksal uygulamayı çalıştırmaya başlayan URL 'YI aramasını bekler. [İstek tetikleyicisine](connectors-native-reqres.md)benzer şekilde, mantıksal uygulama, belirtilen olay gerçekleştiğinde hemen başlatılır. Tetikleyiciyi kaldırır ve mantıksal uygulamanızı kaydettiğinizde veya mantıksal uygulamanızı etkin durumundan devre dışı olarak değiştirdiğinizde tetikleyici, hizmetten veya uç noktadan *aboneliği* kaldırır.

Bir HTTP Web kancası eylemi ayrıca olay tabanlıdır ve bu hizmet veya uç noktaya *geri çağırma URL 'si* kaydederek belirli bir hizmete veya uç noktaya *abone* olur. Web kancası eylemi mantıksal uygulamanın iş akışını duraklatır ve Logic App çalışmaya devam etmeden önce hizmet veya uç nokta URL 'YI çağırana kadar bekler. Eylem mantığı uygulaması, bu durumlarda hizmetten veya uç noktadan *aboneliği* kaldırır:

* Web kancası eylemi başarıyla tamamlandığında
* Bir yanıt beklenirken mantıksal uygulama çalıştırması iptal edilirse
* Mantıksal uygulama zaman aşımına uğramadan önce

Örneğin, Office 365 Outlook bağlayıcısının [**onay e-postası gönder**](connectors-create-api-office365-outlook.md) eylemi, bu kalıbı izleyen bir Web kancası eyleminin örneğidir. Web kancası eylemini kullanarak bu kalıbı herhangi bir hizmete genişletebilirsiniz.

> [!NOTE]
> Logic Apps, geri çağrıyı HTTP Web kancası tetikleyicisine veya eylemine geri alırken Aktarım Katmanı Güvenliği (TLS) 1,2 ' i zorlar. SSL el sıkışma hataları görürseniz, TLS 1,2 kullandığınızdan emin olun. Gelen çağrılar için desteklenen şifre paketleri şunlardır:
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

* [HTTP Web kancası tetikleyici parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)
* [Web kancaları ve abonelikler](../logic-apps/logic-apps-workflow-actions-triggers.md#webhooks-and-subscriptions)
* [Web kancasını destekleyen özel API 'Ler oluşturma](../logic-apps/logic-apps-create-api-app.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantıksal uygulamalar veya Web](../logic-apps/logic-apps-create-api-app.md#webhook-triggers) kancası [eylemleri](../logic-apps/logic-apps-create-api-app.md#webhook-actions) için Web kancası abone ve abonelik kaldırma düzenlerini destekleyen, önceden dağıtılan BIR uç noktanın veya API 'nin URL 'si

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)hakkında temel bilgi. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md) olduğunu gözden geçirin.

* Hedef uç noktada belirli olayları beklemek istediğiniz mantıksal uygulama. HTTP Web kancası tetikleyicisinden başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). HTTP Web kancası eylemini kullanmak için, mantıksal uygulamanızı istediğiniz tetikleyiciyle başlatın. Bu örnek, ilk adım olarak HTTP tetikleyicisini kullanır.

## <a name="add-an-http-webhook-trigger"></a>HTTP Web kancası tetikleyicisi ekleme

Bu yerleşik tetikleyici, hedef hizmette Subscribe uç noktasını çağırır ve hedef hizmete bir geri çağırma URL 'SI kaydeder. Mantıksal uygulamanız daha sonra hedef hizmetin geri çağırma URL 'sine bir `HTTP POST` isteği göndermesini bekler. Bu olay gerçekleştiğinde, tetikleyici ateşlenir ve istekteki tüm verileri iş akışına karşı geçirir.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında boş mantıksal uygulamanızı açın.

1. Tasarımcı 'nın arama kutusuna filtreniz olarak `http webhook` girin. **Tetikleyiciler** listesinden **http Web kancası** tetikleyicisi ' ni seçin.

   ![HTTP Web kancası tetikleyicisi seçin](./media/connectors-native-webhook/select-http-webhook-trigger.png)

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için tetikleyiciyi `HTTP Webhook trigger` olarak yeniden adlandırır. Ayrıca, örnek daha sonra bir HTTP Web kancası eylemi ekler ve her iki ad de benzersiz olmalıdır.

1. Abone ol ve abonelik kaldırma çağrıları için kullanmak istediğiniz [http Web kancası tetikleyici parametrelerinin](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger) değerlerini belirtin.

   Bu örnekte tetikleyici, abone ve abonelik kaldırma işlemleri gerçekleştirilirken kullanılacak yöntemleri, URI 'Leri ve ileti gövdelerini içerir.

   ![HTTP Web kancası tetikleyici parametrelerini girin](./media/connectors-native-webhook/http-webhook-trigger-parameters.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Subscription-yöntemi** | Yes | Hedef uç noktaya abone olurken kullanılacak yöntem |
   | **Abone ol-URI** | Yes | Hedef uç noktaya abone olmak için kullanılacak URL |
   | **Abone ol-gövde** | Hayır | Abonelik isteğine dahil edilecek herhangi bir ileti gövdesi. Bu örnek, mantıksal uygulamanızın geri arama URL 'sini almak için `@listCallbackUrl()` ifadesini kullanarak mantıksal uygulamanız olan aboneyi benzersiz bir şekilde tanımlayan geri çağırma URL 'sini içerir. |
   | **Abonelik kaldırma yöntemi** | Hayır | Hedef uç noktadan abone olunurken kullanılacak yöntem |
   | **Abonelik kaldırma-URI** | Hayır | Hedef uç noktadan abone olunurken kullanılacak URL |
   | **Abonelik kaldırma-gövde** | Hayır | Abonelik kaldırma isteğine eklenecek isteğe bağlı bir ileti gövdesi <p><p>**Note**: bu özellik `listCallbackUrl()` işlevinin kullanımını desteklemiyor. Ancak tetikleyici, hedef hizmetin aboneyi benzersiz şekilde tanımlamak için kullanabileceği `x-ms-client-tracking-id` ve `x-ms-workflow-operation-name`üst bilgilerini otomatik olarak ekler ve gönderir. |
   ||||

1. Başka tetikleyici özellikleri eklemek için **yeni parametre Ekle** listesini açın.

   ![Daha fazla tetikleyici özelliği Ekle](./media/connectors-native-webhook/http-webhook-trigger-add-properties.png)

   Örneğin, kimlik doğrulaması kullanmanız gerekiyorsa, **abone ol-Authentication** ve **abonelikten çıkma-Authentication** özelliklerini ekleyebilirsiniz. HTTP Web kancası için kullanılabilen kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. [giden çağrılara kimlik doğrulama ekleme](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. Tetikleyici tetiklendiğinde çalıştırılan eylemlerle mantıksal uygulamanızın iş akışını oluşturmaya devam edin.

1. İşiniz bittiğinde, işiniz bittiğinde mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

   Mantıksal uygulamanızı kaydetmek, hedef hizmette abone ol uç noktasını çağırır ve geri çağırma URL 'sini kaydeder. Mantıksal uygulamanız daha sonra hedef hizmetin geri çağırma URL 'sine bir `HTTP POST` isteği göndermesini bekler. Bu olay gerçekleştiğinde, tetikleyici ateşlenir ve istekteki tüm verileri iş akışına karşı geçirir. Bu işlem başarıyla tamamlanırsa, tetikleyici, uç noktadan abone olur ve mantıksal uygulamanız kalan iş akışını sürdürür.

## <a name="add-an-http-webhook-action"></a>HTTP Web kancası eylemi ekleme

Bu yerleşik eylem, hedef hizmette abone ol uç noktasını çağırır ve hedef hizmete bir geri çağırma URL 'SI kaydeder. Daha sonra mantıksal uygulamanız duraklatılır ve hedef hizmetin geri çağırma URL 'sine `HTTP POST` isteği göndermesini bekler. Bu olay gerçekleştiğinde eylem, istekteki tüm verileri iş akışına karşı geçirir. İşlem başarıyla tamamlanırsa, eylem uç noktadan aboneliği kaldırır ve mantıksal uygulamanız kalan iş akışını çalıştırmaya devam eder.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

   Bu örnek, ilk adım olarak HTTP Web kancası tetikleyicisini kullanır.

1. HTTP Web kancası eylemini eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Adımlar arasında bir eylem eklemek için, işaretçinizi adımlar arasındaki oka taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Tasarımcı 'nın arama kutusuna filtreniz olarak `http webhook` girin. **Eylemler** listesinden **http Web kancası** eylemini seçin.

   ![HTTP Web kancası eylemini seç](./media/connectors-native-webhook/select-http-webhook-action.png)

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için eylemi "HTTP Web kancası eylemi" olarak yeniden adlandırır.

1. Abone ve abonelik kaldırma çağrıları için kullanmak istediğiniz http Web kancası [tetikleyici parametrelerine](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)benzer http Web kancası eylem parametreleri için değerler sağlayın.

   Bu örnekte, eylem abone ve abonelik kaldırma işlemleri gerçekleştirilirken kullanılacak yöntemleri, URI 'Leri ve ileti gövdelerini içerir.

   ![HTTP Web kancası eylem parametrelerini girin](./media/connectors-native-webhook/http-webhook-action-parameters.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Subscription-yöntemi** | Yes | Hedef uç noktaya abone olurken kullanılacak yöntem |
   | **Abone ol-URI** | Yes | Hedef uç noktaya abone olmak için kullanılacak URL |
   | **Abone ol-gövde** | Hayır | Abonelik isteğine dahil edilecek herhangi bir ileti gövdesi. Bu örnek, mantıksal uygulamanızın geri arama URL 'sini almak için `@listCallbackUrl()` ifadesini kullanarak mantıksal uygulamanız olan aboneyi benzersiz bir şekilde tanımlayan geri çağırma URL 'sini içerir. |
   | **Abonelik kaldırma yöntemi** | Hayır | Hedef uç noktadan abone olunurken kullanılacak yöntem |
   | **Abonelik kaldırma-URI** | Hayır | Hedef uç noktadan abone olunurken kullanılacak URL |
   | **Abonelik kaldırma-gövde** | Hayır | Abonelik kaldırma isteğine eklenecek isteğe bağlı bir ileti gövdesi <p><p>**Note**: bu özellik `listCallbackUrl()` işlevinin kullanımını desteklemiyor. Ancak eylem, hedef hizmetin aboneyi benzersiz şekilde tanımlamak için kullanabileceği `x-ms-client-tracking-id` ve `x-ms-workflow-operation-name`üst bilgilerini otomatik olarak ekler ve gönderir. |
   ||||

1. Başka eylem özellikleri eklemek için **yeni parametre Ekle** listesini açın.

   ![Daha fazla eylem özelliği ekleyin](./media/connectors-native-webhook/http-webhook-action-add-properties.png)

   Örneğin, kimlik doğrulaması kullanmanız gerekiyorsa, **abone ol-Authentication** ve **abonelikten çıkma-Authentication** özelliklerini ekleyebilirsiniz. HTTP Web kancası için kullanılabilen kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. [giden çağrılara kimlik doğrulama ekleme](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. İşiniz bittiğinde mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

   Bu eylem çalıştırıldığında, mantıksal uygulamanız hedef hizmette abone ol uç noktasını çağırır ve geri arama URL 'sini kaydeder. Mantıksal uygulama daha sonra iş akışını duraklatır ve hedef hizmetin geri çağırma URL 'sine `HTTP POST` isteği göndermesini bekler. Bu olay gerçekleştiğinde eylem, istekteki tüm verileri iş akışına karşı geçirir. İşlem başarıyla tamamlanırsa, eylem uç noktadan aboneliği kaldırır ve mantıksal uygulamanız kalan iş akışını çalıştırmaya devam eder.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyici ve eylem parametreleri hakkında daha fazla bilgi için, bkz. [http Web kancası parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger).

### <a name="output-details"></a>Çıkış ayrıntıları

Bu bilgileri döndüren bir HTTP Web kancası tetikleyicisinden veya eyleminden alınan çıktılar hakkında daha fazla bilgi bulabilirsiniz:

| Özellik adı | Tür | Açıklama |
|---------------|------|-------------|
| üstbilgiler | object | İstekten gelen üstbilgiler |
| body | object | JSON nesnesi | İstekten gelen gövde içeriğine sahip nesne |
| durum kodu | int | İstekteki durum kodu |
|||

| Durum kodu | Açıklama |
|-------------|-------------|
| 200 | Tamam |
| 202 | Eden |
| 400 | Hatalı istek |
| 401 | Yetkisiz |
| 403 | Yasak |
| 404 | Bulunamadı |
| 500 | İç sunucu hatası. Bilinmeyen bir hata oluştu. |
|||

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
