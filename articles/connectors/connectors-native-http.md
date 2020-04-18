---
title: HTTP veya HTTPS kullanarak hizmet bitiş noktalarını arayın
description: Azure Logic Apps'tan hizmet bitiş noktalarına giden HTTP veya HTTPS isteklerini gönderme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/12/2020
tags: connectors
ms.openlocfilehash: 6c52f2df34faf441ab70b48b11bbc393ebcecb65
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617617"
---
# <a name="call-service-endpoints-over-http-or-https-from-azure-logic-apps"></a>Azure Logic Apps'tan HTTP veya HTTPS üzerinden hizmet bitiş noktalarını arama

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik HTTP tetikleyicisi veya eylemi yle, http veya HTTPS üzerinden hizmet bitiş noktalarına istek gönderen otomatik görevler ve iş akışları oluşturabilirsiniz. Örneğin, belirli bir zamanlamada bu bitiş noktasını denetleyerek web sitenizin hizmet bitiş noktasını izleyebilirsiniz. Belirtilen olay, web sitenizin çöküşü gibi bu uç noktada gerçekleştiğinde, olay mantık uygulamanızın iş akışını tetikler ve bu iş akışındaki eylemleri çalıştırAr. Bunun yerine gelen HTTPS çağrılarını almak ve bu çağrıları yanıtlamak istiyorsanız, yerleşik [İstek tetikleyicisini veya Yanıt eylemini](../connectors/connectors-native-reqres.md)kullanın.

> [!NOTE]
> Hedef uç noktanın özelliğini temel alan HTTP bağlayıcısı, Aktarım Katmanı Güvenliği (TLS) 1.0, 1.1 ve 1.2 sürümlerini destekler. Logic Apps, mümkün olan en yüksek desteklenen sürümü kullanarak bitiş noktası yla görüşür. Örneğin, uç nokta 1.2'yi destekliyorsa, bağlayıcı önce 1.2'yi kullanır. Aksi takdirde, bağlayıcı sonraki en yüksek desteklenen sürümü kullanır.

Yinelenen bir zamanlamada bir bitiş noktasını denetlemek veya *yoklamak* için, iş akışınızın ilk adımı olarak [HTTP tetikleyicisini ekleyin.](#http-trigger) Tetikleyici bitiş noktasını her denetlesin, tetikleyici çağıran veya bitiş noktasına bir *istek* gönderir. Bitiş noktasının yanıtı, mantık uygulamanızın iş akışının çalışıp çalışmadığını belirler. Tetikleyici, mantık uygulamanızdaki eylemlere uç noktanın yanıtından herhangi bir içeriği geçirir.

İş akışınızdaki herhangi bir yerden bir bitiş noktası çağırmak için [HTTP eylemini ekleyin.](#http-action) Bitiş noktasının yanıtı, iş akışınızın kalan eylemlerinin nasıl çalıştığını belirler.

> [!IMPORTANT]
> Bir HTTP tetikleyicisi veya eylemi bu üstbilgi içeriyorsa, Logic Apps bu üstbilgilerini oluşturulan istek iletisinden herhangi bir uyarı veya hata göstermeden kaldırır:
>
> * `Accept-*`
> * `Allow`
> * `Content-*`bu istisnalar `Content-Disposition`dışında: , `Content-Encoding`, ve`Content-Type`
> * `Cookie`
> * `Expires`
> * `Host`
> * `Last-Modified`
> * `Origin`
> * `Set-Cookie`
> * `Transfer-Encoding`
>
> Logic Apps, http tetikleyicisi kullanan mantıksal uygulamaları kaydetmenizi veya bu üstbilgilerle eylemde bulunmanızı engellemese de, Logic Apps bu üstbilgi zamalarını yok sayar.

Bu makalede, mantık uygulamanızın iş akışına bir HTTP tetikleyicisi veya eylem nasıl ekleyeceğiniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Aramak istediğiniz hedef bitiş noktasının URL'si

* [Mantık uygulamaları oluşturmak için nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md)temel bilgi . Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi](../logic-apps/logic-apps-overview.md)inceleyin?

* Hedef bitiş noktasını aramak istediğiniz yerden gelen mantık uygulaması. HTTP tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) HTTP eylemini kullanmak için mantık uygulamanızı istediğiniz tetikleyiciyle başlatın. Bu örnekte ilk adım olarak HTTP tetikleyicisi kullanır.

<a name="http-trigger"></a>

## <a name="add-an-http-trigger"></a>HTTP tetikleyicisi ekleme

Bu yerleşik tetikleyici, bitiş noktası için belirtilen URL'ye BIR HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş mantık uygulamanızı Logic App Designer'da açın.

1. Tasarımcının arama kutusunun altında **Yerleşik'i**seçin. Arama kutusuna filtreniz olarak girin. `http` **Tetikleyiciler** listesinden **HTTP** tetikleyicisini seçin.

   ![HTTP tetikleyiciyi seçin](./media/connectors-native-http/select-http-trigger.png)

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için tetikleyiciyi "HTTP tetikleyicisi" olarak yeniden adlandırır. Ayrıca, örnek daha sonra bir HTTP eylemi ekler ve her iki ad benzersiz olmalıdır.

1. Hedef bitiş noktasına çağrıya eklemek istediğiniz [HTTP tetikleyici parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger) için değerleri sağlayın. Tetikleyicinin hedef bitiş noktasını denetlemesini ne sıklıkta istediğinize göre yinelemeyi ayarlayın.

   ![HTTP tetikleme parametrelerini girin](./media/connectors-native-http/http-trigger-parameters.png)

   **Yok**dışında bir kimlik doğrulama türü seçerseniz, kimlik doğrulama ayarları seçiminize göre değişir. HTTP için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için şu konulara bakın:

   * [Giden aramalara kimlik doğrulama ekleme](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
   * [Yönetilen kimliklerle kaynaklara erişimi doğrulama](../logic-apps/create-managed-service-identity.md)

1. Kullanılabilir diğer parametrelereklemek için **yeni parametre** ekle listesini açın ve istediğiniz parametreleri seçin.

1. Tetikleyici ateşlendiğinde çalışan eylemlerle mantık uygulamanızın iş akışını oluşturmaya devam edin.

1. Işiniz bittiğinde, mantık uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

<a name="http-action"></a>

## <a name="add-an-http-action"></a>HTTP eylemi ekleme

Bu yerleşik eylem, bitiş noktası için belirtilen URL'ye BIR HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte ilk adım olarak HTTP tetikleyicisi kullanır.

1. HTTP eylemini eklemek istediğiniz adımın altında **Yeni adımı**seçin.

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Bir **eylem seçin,** **Dahili'yi**seçin. Arama kutusuna filtreniz olarak girin. `http` **Eylemler** listesinden **HTTP** eylemini seçin.

   ![HTTP eylemini seçin](./media/connectors-native-http/select-http-action.png)

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için eylemi "HTTP eylemi" olarak yeniden adlandırır.

1. Hedef bitiş noktasına çağrıya eklemek istediğiniz [HTTP eylem parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action) için değerleri sağlayın.

   ![HTTP eylem parametrelerini girin](./media/connectors-native-http/http-action-parameters.png)

   **Yok**dışında bir kimlik doğrulama türü seçerseniz, kimlik doğrulama ayarları seçiminize göre değişir. HTTP için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için şu konulara bakın:

   * [Giden aramalara kimlik doğrulama ekleme](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
   * [Yönetilen kimliklerle kaynaklara erişimi doğrulama](../logic-apps/create-managed-service-identity.md)

1. Kullanılabilir diğer parametrelereklemek için **yeni parametre** ekle listesini açın ve istediğiniz parametreleri seçin.

1. Işiniz bittiğinde, mantık uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

## <a name="content-with-multipartform-data-type"></a>Çok parçalı/form-veri türüne sahip içerik

HTTP isteklerinde `multipart/form-data` türü olan içeriği işlemek için, bu biçimi `$content-type` kullanarak `$multipart` HTTP isteğinin gövdesine öznitelikleri ve öznitelikleri içeren bir JSON nesnesi ekleyebilirsiniz.

```json
"body": {
   "$content-type": "multipart/form-data",
   "$multipart": [
      {
         "body": "<output-from-trigger-or-previous-action>",
         "headers": {
            "Content-Disposition": "form-data; name=file; filename=<file-name>"
         }
      }
   ]
}
```

Örneğin, bir Excel dosyası için HTTP POST isteği gönderen bir mantık uygulamanız olduğunu varsayalım ve bu `multipart/form-data` sitenin türünü destekleyen API'sini kullanarak bir web sitesine gönderin. Bu eylem şu şekilde görünebilir:

![Çok parçalı form verileri](./media/connectors-native-http/http-action-multipart.png)

Aşağıda, temel iş akışı tanımında HTTP eyleminin JSON tanımını gösteren aynı örnek verilmiştir:

```json
"HTTP_action": {
   "inputs": {
      "body": {
         "$content-type": "multipart/form-data",
         "$multipart": [
            {
               "body": "@trigger()",
               "headers": {
                  "Content-Disposition": "form-data; name=file; filename=myExcelFile.xlsx"
               }
            }
         ]
      },
      "method": "POST",
      "uri": "https://finance.contoso.com"
   },
   "runAfter": {},
   "type": "Http"
}
```

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyici ve eylem parametreleri hakkında daha fazla bilgi için aşağıdaki bölümlere bakın:

* [HTTP tetik parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger)
* [HTTP eylem parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)

### <a name="output-details"></a>Çıktı ayrıntıları

Bu bilgileri döndüren bir HTTP tetikleyicisinden veya eyleminden çıkan çıktılar hakkında daha fazla bilgi aşağıda veda edebilirsiniz:

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
