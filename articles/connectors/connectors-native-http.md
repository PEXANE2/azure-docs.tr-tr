---
title: HTTP veya HTTPS kullanarak hizmet uç noktalarını çağırma
description: Azure Logic Apps 'den hizmet uç noktalarına giden HTTP veya HTTPS istekleri gönderme
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: conceptual
ms.date: 05/29/2020
tags: connectors
ms.openlocfilehash: 33075173385a6e36829199c5bda854c78a4424fc
ms.sourcegitcommit: 58ff2addf1ffa32d529ee9661bbef8fbae3cddec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84325125"
---
# <a name="call-service-endpoints-over-http-or-https-from-azure-logic-apps"></a>Azure Logic Apps 'den HTTP veya HTTPS üzerinden hizmet uç noktalarını çağır

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik http tetikleyicisi veya EYLEMIYLE, http veya https üzerinden hizmet uç noktalarına istek gönderen otomatikleştirilmiş görevler ve iş akışları oluşturabilirsiniz. Örneğin, bu uç noktayı belirli bir zamanlamaya göre denetleyerek Web siteniz için hizmet uç noktasını izleyebilirsiniz. Bu uç noktada belirtilen olay, Web siteniz gibi olduğunda, olay mantıksal uygulamanızın iş akışını tetikler ve bu iş akışındaki eylemleri çalıştırır. Bunun yerine gelen HTTPS çağrılarını alıp yanıtlamak istiyorsanız, yerleşik [istek tetikleyicisi veya Yanıt eylemini](../connectors/connectors-native-reqres.md)kullanın.

* Bir uç noktayı yinelenen bir zamanlamaya göre denetlemek veya *yoklamak* IÇIN, [http tetikleyicisini](#http-trigger) iş akışınıza ilk adım olarak ekleyin. Tetikleyici uç noktayı her denetlediğinde tetikleyici, uç noktaya bir istek çağırır veya bir *istek* gönderir. Uç noktanın yanıtı, mantıksal uygulamanızın iş akışının çalışıp çalışmadığını belirler. Tetikleyici, mantıksal uygulamanızdaki eylemlere bitiş noktası yanıtından içerik geçirir.

* Bir uç noktayı iş akışınızda başka herhangi bir yerde çağırmak için [http eylemini ekleyin](#http-action). Uç noktanın yanıtı, iş akışınızın kalan eylemlerinin nasıl çalışacağını belirler.

Bu makalede mantıksal uygulamanızın iş akışına bir HTTP tetikleyicisi veya eyleminin nasıl ekleneceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Çağırmak istediğiniz hedef uç noktanın URL 'SI

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)hakkında temel bilgi. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md)olduğunu gözden geçirin.

* Hedef uç noktayı çağırmak istediğiniz mantıksal uygulama. HTTP tetikleyicisiyle başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). HTTP eylemini kullanmak için mantıksal uygulamanızı istediğiniz tetikleyiciyle başlatın. Bu örnek, ilk adım olarak HTTP tetikleyicisini kullanır.

<a name="tls-support"></a>

## <a name="transport-layer-security-tls"></a>Aktarım Katmanı Güvenliği (TLS)

Hedef uç noktanın özelliği temel alınarak, giden çağrılar daha önce Güvenli Yuva Katmanı (SSL), sürüm 1,0, 1,1 ve 1,2 olan Aktarım Katmanı Güvenliği 'ni (TLS) destekler. Logic Apps, uç nokta ile mümkün olan en yüksek desteklenen sürümü kullanarak görüşür.

Örneğin, uç nokta 1,2 destekliyorsa, HTTP Bağlayıcısı önce 1,2 ' i kullanır. Aksi halde, bağlayıcı desteklenen bir sonraki en yüksek sürümü kullanır.

<a name="self-signed"></a>

## <a name="self-signed-certificates"></a>Otomatik olarak imzalanan sertifikalar

* Genel, çok kiracılı Azure ortamındaki Logic Apps için, HTTP Bağlayıcısı kendinden imzalı TLS/SSL sertifikalarına izin vermez. Mantıksal uygulamanız sunucuya HTTP çağrısı yapar ve bir TLS/SSL otomatik olarak imzalanan sertifika sunduğunda, HTTP çağrısı hata vererek başarısız olur `TrustFailure` .

* Bir [tümleştirme hizmeti ortamındaki (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)Logic Apps için http BAĞLAYıCıSı, TLS/SSL el sıkışmaları için otomatik olarak imzalanan sertifikalara izin verir. Ancak, önce Logic Apps REST API kullanarak mevcut bir ıSE veya yeni bir ıSE için [otomatik olarak imzalanan sertifika desteğini etkinleştirmeniz](../logic-apps/create-integration-service-environment-rest-api.md#request-body) ve ortak sertifikayı konuma yüklemeniz gerekir `TrustedRoot` .

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="omitted-http-headers"></a>Atlanan HTTP üstbilgileri

Bir HTTP tetikleyicisi veya eylemi bu üstbilgileri içeriyorsa Logic Apps, bu üst bilgileri herhangi bir uyarı veya hata görüntülenmeden oluşturulan istek iletisinden kaldırır:

* `Accept-*`
* `Allow`
* `Content-*`Bu özel durumlarla birlikte: `Content-Disposition` , `Content-Encoding` ve`Content-Type`
* `Cookie`
* `Expires`
* `Host`
* `Last-Modified`
* `Origin`
* `Set-Cookie`
* `Transfer-Encoding`

Logic Apps, bu üst bilgilerle bir HTTP tetikleyicisi veya eylemi kullanan mantıksal uygulamaları kaydetmenizi durdurmasa da, Logic Apps bu üst bilgileri yoksayar.

<a name="http-trigger"></a>

## <a name="add-an-http-trigger"></a>HTTP tetikleyicisi ekleme

Bu yerleşik tetikleyici, bir uç nokta için belirtilen URL 'ye HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure portalında](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında boş mantıksal uygulamanızı açın.

1. Tasarımcı 'nın arama kutusunda **yerleşik**' i seçin. Arama kutusuna `http` filtreniz olarak yazın. **Tetikleyiciler** listesinden **http** tetikleyicisi ' ni seçin.

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

1. [Azure portalında](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

   Bu örnek, ilk adım olarak HTTP tetikleyicisini kullanır.

1. HTTP eylemini eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Adımlar arasında bir eylem eklemek için, işaretçinizi adımlar arasındaki oka taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. **Eylem seçin**altında, **yerleşik**' i seçin. Arama kutusuna `http` filtreniz olarak yazın. **Eylemler** listesinden **http** eylemini seçin.

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

HTTP isteklerinde türü olan içeriği işlemek için `multipart/form-data` , `$content-type` `$multipart` Bu biçimi kullanarak http isteğinin gövdesine ve ÖZNITELIKLERINI içeren bir JSON nesnesi ekleyebilirsiniz.

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

Örneğin, bu sitenin API 'sini kullanarak bir Web sitesine bir Excel dosyası için HTTP POST isteği gönderen bir mantıksal uygulamanız olduğunu varsayalım `multipart/form-data` . Bu eylemin şu şekilde görünebileceğini aşağıda görebilirsiniz:

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

| Özellik adı | Tür | Description |
|---------------|------|-------------|
| bilgisinde | nesne | İstekten gelen üstbilgiler |
| body | nesne | JSON nesnesi | İstekten gelen gövde içeriğine sahip nesne |
| durum kodu | int | İstekteki durum kodu |
|||

| Durum kodu | Description |
|-------------|-------------|
| 200 | Tamam |
| 202 | Kabul edildi |
| 400 | Hatalı istek |
| 401 | Yetkisiz |
| 403 | Yasak |
| 404 | Bulunamadı |
| 500 | İç sunucu hatası. Bilinmeyen bir hata oluştu. |
|||

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
