---
title: Azure Logic Apps 'ten HTTP veya HTTPS uç noktalarına bağlanma
description: Azure Logic Apps kullanarak otomatik görevler, süreçler ve iş akışlarında HTTP veya HTTPS uç noktalarını izleyin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/05/2019
tags: connectors
ms.openlocfilehash: 04d9beaef29e76d40c0bb3f9dcf0bb6f4fe3152d
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234357"
---
# <a name="call-http-or-https-endpoints-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak HTTP veya HTTPS uç noktalarını çağırma

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik http Bağlayıcısı sayesinde mantıksal uygulamalar oluşturarak HERHANGI bir http veya HTTPS uç noktasını düzenli olarak çağıran iş akışlarını otomatikleştirebilirsiniz. Örneğin, belirli bir zamanlamaya göre bu uç noktayı denetleyerek Web siteniz için hizmet uç noktasını izleyebilirsiniz. Bu uç noktada ilgili Web siteniz gibi belirli bir olay gerçekleştiğinde, olay mantıksal uygulamanızın iş akışını tetikler ve belirtilen eylemleri çalıştırır.

Bir uç noktayı düzenli bir zamanlamaya göre denetlemek veya *yoklamak* IÇIN, http tetikleyicisini iş akışınızın ilk adımı olarak kullanabilirsiniz. Her denetim için tetikleyici, uç noktaya bir çağrı veya *istek* gönderir. Uç noktanın yanıtı, mantıksal uygulamanızın iş akışının çalışıp çalışmadığını belirler. Tetikleyici, mantıksal uygulamanızdaki eylemlere karşılık gelen tüm içerik üzerinde geçiş yapar.

HTTP eylemini, istediğiniz zaman uç noktayı çağırmak için iş akışınızda başka herhangi bir adım olarak kullanabilirsiniz. Uç noktanın yanıtı, iş akışınızın kalan eylemlerinin nasıl çalışacağını belirler.

Hedef uç noktanın özelliği temel alınarak, HTTP Bağlayıcısı Aktarım Katmanı Güvenliği (TLS) 1,0, 1,1 ve 1,2 sürümlerini destekler. Logic Apps, uç nokta ile mümkün olan en yüksek desteklenen sürümü kullanarak görüşür. Bu nedenle, örneğin, uç nokta 1,2 ' ı destekliyorsa, bağlayıcı önce 1,2 ' i kullanır. Aksi halde, bağlayıcı desteklenen bir sonraki en yüksek sürümü kullanır.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Çağırmak istediğiniz hedef uç noktanın URL 'SI

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)hakkında temel bilgi. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md) olduğunu gözden geçirin.

* Hedef uç noktayı çağırmak istediğiniz mantıksal uygulama. HTTP tetikleyicisiyle başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). HTTP eylemini kullanmak için mantıksal uygulamanızı istediğiniz tetikleyiciyle başlatın. Bu örnek, ilk adım olarak HTTP tetikleyicisini kullanır.

## <a name="add-an-http-trigger"></a>HTTP tetikleyicisi ekleme

Bu yerleşik tetikleyici, bir uç nokta için belirtilen URL 'ye HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında boş mantıksal uygulamanızı açın.

1. Tasarımcıda arama kutusuna filtreniz olarak "http" yazın. **Tetikleyiciler** listesinden **http** tetikleyicisi ' ni seçin.

   ![HTTP tetikleyicisi seçin](./media/connectors-native-http/select-http-trigger.png)

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için tetikleyiciyi "HTTP tetikleyicisi" olarak yeniden adlandırır. Ayrıca, örnek daha sonra bir HTTP eylemi ekler ve her iki ad de benzersiz olmalıdır.

1. Hedef uç noktaya çağrısına dahil etmek istediğiniz [http tetikleyici parametrelerinin](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger) değerlerini belirtin. Tetikleyicinin hedef uç noktayı denetlemesini istediğiniz sıklık için yinelemeyi ayarlayın.

   ![HTTP tetikleyici parametrelerini girin](./media/connectors-native-http/http-trigger-parameters.png)

   HTTP için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. [http Tetikleyicileri ve eylemlerini kimlik](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication)doğrulama.

1. Kullanılabilir başka parametreler eklemek için **yeni parametre Ekle** listesini açın ve istediğiniz parametreleri seçin.

1. Tetikleyici tetiklendiğinde çalıştırılan eylemlerle mantıksal uygulamanızın iş akışını oluşturmaya devam edin.

1. İşiniz bittiğinde, işiniz bittiğinde mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

## <a name="add-an-http-action"></a>HTTP eylemi ekleme

Bu yerleşik eylem, bir uç nokta için belirtilen URL 'ye HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

   Bu örnek, ilk adım olarak HTTP tetikleyicisini kullanır.

1. HTTP eylemini eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Adımlar arasında bir eylem eklemek için, işaretçinizi adımlar arasındaki oka taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Tasarımcıda arama kutusuna filtreniz olarak "http" yazın. **Eylemler** listesinden **http** eylemini seçin.

   ![HTTP eylemi seçin](./media/connectors-native-http/select-http-action.png)

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için eylemi "HTTP eylemi" olarak yeniden adlandırır.

1. Hedef uç noktaya çağrısına dahil etmek istediğiniz [http eylem parametrelerinin](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action) değerlerini belirtin.

   ![HTTP eylem parametrelerini girin](./media/connectors-native-http/http-action-parameters.png)

   HTTP için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. [http Tetikleyicileri ve eylemlerini kimlik](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication)doğrulama.

1. Kullanılabilir başka parametreler eklemek için **yeni parametre Ekle** listesini açın ve istediğiniz parametreleri seçin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

## <a name="content-with-multipartform-data-type"></a>Multipart/form veri türü olan içerik

Http isteklerinde `multipart/form-data` türü olan içeriği işlemek için, bu biçimi kullanarak http isteğinin gövdesine `$content-type` ve `$multipart` özniteliklerini içeren bir JSON nesnesi ekleyebilirsiniz.

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

Örneğin, bu sitenin API `multipart/form-data` 'sini kullanarak bir Web sitesine bir Excel dosyası için http post isteği gönderen bir mantıksal uygulamanız olduğunu varsayalım. Bu eylemin şu şekilde görünebileceğini aşağıda görebilirsiniz:

![Çok parçalı form verileri](./media/connectors-native-http/http-action-multipart.png)

Aşağıda, temel alınan iş akışı tanımında HTTP eyleminin JSON tanımını gösteren aynı örnek verilmiştir:

```json
{
   "HTTP_action": {
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

* [HTTP tetikleyici parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger)
* [HTTP eylem parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action)

### <a name="output-details"></a>Çıkış ayrıntıları

Bu bilgileri döndüren bir HTTP tetikleyicisinden veya eylemden çıktılar hakkında daha fazla bilgi bulabilirsiniz:

| Özellik adı | Type | Açıklama |
|---------------|------|-------------|
| Bilgisinde | object | İstekten gelen üstbilgiler |
| bölümü | object | JSON nesnesi | İstekten gelen gövde içeriğine sahip nesne |
| Durum kodu | int | İstekteki durum kodu |
|||

| Durum kodu | Açıklama |
|-------------|-------------|
| 200 | Tamam |
| 202 | Eden |
| 400 | Hatalı istek |
| 401 | Yetkilendirilmemiş |
| 403 | Yasak |
| 404 | Bulunamadı |
| 500 | İç sunucu hatası. Bilinmeyen bir hata oluştu. |
|||

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
