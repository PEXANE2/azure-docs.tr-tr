---
title: HTTP veya HTTPS kullanarak hizmet uç noktalarını çağırma
description: Azure Logic Apps 'den hizmet uç noktalarına giden HTTP veya HTTPS istekleri gönderme
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: conceptual
ms.date: 03/12/2020
tags: connectors
ms.openlocfilehash: 9ed3d960b3f5653ea8706b39559c9d5a71c45a6c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81867626"
---
# <a name="call-service-endpoints-over-http-or-https-from-azure-logic-apps"></a>Azure Logic Apps 'den HTTP veya HTTPS üzerinden hizmet uç noktalarını çağır

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik http tetikleyicisi veya EYLEMIYLE, http veya https üzerinden hizmet uç noktalarına istek gönderen otomatikleştirilmiş görevler ve iş akışları oluşturabilirsiniz. Örneğin, bu uç noktayı belirli bir zamanlamaya göre denetleyerek Web siteniz için hizmet uç noktasını izleyebilirsiniz. Bu uç noktada belirtilen olay, Web siteniz gibi olduğunda, olay mantıksal uygulamanızın iş akışını tetikler ve bu iş akışındaki eylemleri çalıştırır. Bunun yerine gelen HTTPS çağrılarını alıp yanıtlamak istiyorsanız, yerleşik [istek tetikleyicisi veya Yanıt eylemini](../connectors/connectors-native-reqres.md)kullanın.

> [!NOTE]
> Hedef uç noktanın özelliği temel alınarak, HTTP Bağlayıcısı Aktarım Katmanı Güvenliği (TLS) 1,0, 1,1 ve 1,2 sürümlerini destekler. Logic Apps, uç nokta ile mümkün olan en yüksek desteklenen sürümü kullanarak görüşür. Örneğin, uç nokta 1,2 ' ı destekliyorsa, bağlayıcı önce 1,2 ' i kullanır. Aksi halde, bağlayıcı desteklenen bir sonraki en yüksek sürümü kullanır.
>
> HTTP Bağlayıcısı, kimlik doğrulaması için ara TLS/SSL sertifikalarını desteklemez.

Bir uç noktayı yinelenen bir zamanlamaya göre denetlemek veya *yoklamak* IÇIN, [http tetikleyicisini](#http-trigger) iş akışınıza ilk adım olarak ekleyin. Tetikleyici uç noktayı her denetlediğinde tetikleyici, uç noktaya bir istek çağırır veya bir *istek* gönderir. Uç noktanın yanıtı, mantıksal uygulamanızın iş akışının çalışıp çalışmadığını belirler. Tetikleyici, mantıksal uygulamanızdaki eylemlere bitiş noktası yanıtından içerik geçirir.

Bir uç noktayı iş akışınızda başka herhangi bir yerde çağırmak için [http eylemini ekleyin](#http-action). Uç noktanın yanıtı, iş akışınızın kalan eylemlerinin nasıl çalışacağını belirler.

> [!IMPORTANT]
> Bir HTTP tetikleyicisi veya eylemi bu üstbilgileri içeriyorsa Logic Apps, bu üst bilgileri herhangi bir uyarı veya hata görüntülenmeden oluşturulan istek iletisinden kaldırır:
>
> * `Accept-*`
> * `Allow`
> * `Content-*`Bu özel durumlarla birlikte `Content-Disposition`: `Content-Encoding`, ve`Content-Type`
> * `Cookie`
> * `Expires`
> * `Host`
> * `Last-Modified`
> * `Origin`
> * `Set-Cookie`
> * `Transfer-Encoding`
>
> Logic Apps, bu üst bilgilerle bir HTTP tetikleyicisi veya eylemi kullanan mantıksal uygulamaları kaydetmenizi durdurmasa da, Logic Apps bu üst bilgileri yoksayar.

Bu makalede mantıksal uygulamanızın iş akışına bir HTTP tetikleyicisi veya eyleminin nasıl ekleneceği gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Çağırmak istediğiniz hedef uç noktanın URL 'SI

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)hakkında temel bilgi. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md)olduğunu gözden geçirin.

* Hedef uç noktayı çağırmak istediğiniz mantıksal uygulama. HTTP tetikleyicisiyle başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). HTTP eylemini kullanmak için mantıksal uygulamanızı istediğiniz tetikleyiciyle başlatın. Bu örnek, ilk adım olarak HTTP tetikleyicisini kullanır.

<a name="http-trigger"></a>

## <a name="add-an-http-trigger"></a>HTTP tetikleyicisi ekleme

Bu yerleşik tetikleyici, bir uç nokta için belirtilen URL 'ye HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında boş mantıksal uygulamanızı açın.

1. Tasarımcı 'nın arama kutusunda **yerleşik**' i seçin. Arama kutusuna filtreniz olarak yazın `http` . **Tetikleyiciler** listesinden **http** tetikleyicisi ' ni seçin.

   ![HTTP tetikleyicisi seçin](./media/connectors-native-http/select-http-trigger.png)

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için tetikleyiciyi "HTTP tetikleyicisi" olarak yeniden adlandırır. Ayrıca, örnek daha sonra bir HTTP eylemi ekler ve her iki ad de benzersiz olmalıdır.

1. Hedef uç noktaya çağrısına dahil etmek istediğiniz [http tetikleyici parametrelerinin](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger) değerlerini belirtin. Tetikleyicinin hedef uç noktayı denetlemesini istediğiniz sıklık için yinelemeyi ayarlayın.

   ![HTTP tetikleyici parametrelerini girin](./media/connectors-native-http/http-trigger-parameters.png)

   **Hiçbiri**dışında bir kimlik doğrulama türü seçerseniz, kimlik doğrulama ayarları seçiminize göre farklılık gösterir. HTTP için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için şu konulara bakın:

   * [Giden çağrılara kimlik doğrulama ekleme](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
   * [Yönetilen kimlikler ile kaynaklara erişimin kimliğini doğrulama](../logic-apps/create-managed-service-identity.md)

1. Kullanılabilir başka parametreler eklemek için **yeni parametre Ekle** listesini açın ve istediğiniz parametreleri seçin.

1. Tetikleyici tetiklendiğinde çalıştırılan eylemlerle mantıksal uygulamanızın iş akışını oluşturmaya devam edin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

<a name="http-action"></a>

## <a name="add-an-http-action"></a>HTTP eylemi ekleme

Bu yerleşik eylem, bir uç nokta için belirtilen URL 'ye HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

   Bu örnek, ilk adım olarak HTTP tetikleyicisini kullanır.

1. HTTP eylemini eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Adımlar arasında bir eylem eklemek için, işaretçinizi adımlar arasındaki oka taşıyın. Görüntülenen artı işaretini (**+**) seçin ve ardından **Eylem Ekle**' yi seçin.

1. **Eylem seçin**altında, **yerleşik**' i seçin. Arama kutusuna filtreniz olarak yazın `http` . **Eylemler** listesinden **http** eylemini seçin.

   ![HTTP eylemi seçin](./media/connectors-native-http/select-http-action.png)

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için eylemi "HTTP eylemi" olarak yeniden adlandırır.

1. Hedef uç noktaya çağrısına dahil etmek istediğiniz [http eylem parametrelerinin](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action) değerlerini belirtin.

   ![HTTP eylem parametrelerini girin](./media/connectors-native-http/http-action-parameters.png)

   **Hiçbiri**dışında bir kimlik doğrulama türü seçerseniz, kimlik doğrulama ayarları seçiminize göre farklılık gösterir. HTTP için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için şu konulara bakın:

   * [Giden çağrılara kimlik doğrulama ekleme](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
   * [Yönetilen kimlikler ile kaynaklara erişimin kimliğini doğrulama](../logic-apps/create-managed-service-identity.md)

1. Kullanılabilir başka parametreler eklemek için **yeni parametre Ekle** listesini açın ve istediğiniz parametreleri seçin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

## <a name="content-with-multipartform-data-type"></a>Multipart/form veri türü olan içerik

HTTP isteklerinde `multipart/form-data` türü olan içeriği işlemek için, bu BIÇIMI kullanarak http isteğinin gövdesine `$content-type` ve `$multipart` özniteliklerini içeren bir JSON nesnesi ekleyebilirsiniz.

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

Örneğin, bu sitenin API `multipart/form-data` 'sini kullanarak bir Web sitesine bir Excel dosyası IÇIN http post isteği gönderen bir mantıksal uygulamanız olduğunu varsayalım. Bu eylemin şu şekilde görünebileceğini aşağıda görebilirsiniz:

![Çok parçalı form verileri](./media/connectors-native-http/http-action-multipart.png)

Aşağıda, temel alınan iş akışı tanımında HTTP eyleminin JSON tanımını gösteren aynı örnek verilmiştir:

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

* [HTTP tetikleyici parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger)
* [HTTP eylem parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)

### <a name="output-details"></a>Çıkış ayrıntıları

Bu bilgileri döndüren bir HTTP tetikleyicisinden veya eylemden çıktılar hakkında daha fazla bilgi bulabilirsiniz:

| Özellik adı | Tür | Açıklama |
|---------------|------|-------------|
| bilgisinde | object | İstekten gelen üstbilgiler |
| body | object | JSON nesnesi | İstekten gelen gövde içeriğine sahip nesne |
| durum kodu | int | İstekteki durum kodu |
|||

| Durum kodu | Açıklama |
|-------------|-------------|
| 200 | Tamam |
| 202 | Accepted |
| 400 | Hatalı istek |
| 401 | Yetkisiz |
| 403 | Yasak |
| 404 | Bulunamadı |
| 500 | İç sunucu hatası. Bilinmeyen bir hata oluştu. |
|||

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
