---
title: HTTP veya HTTPS kullanarak hizmet uç noktalarını çağırma
description: Azure Logic Apps 'den hizmet uç noktalarına giden HTTP veya HTTPS istekleri gönderme
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: conceptual
ms.date: 06/09/2020
tags: connectors
ms.openlocfilehash: 8c7a0ddb80ba28548fc1821cc2063e500af0fa66
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286640"
---
# <a name="call-service-endpoints-over-http-or-https-from-azure-logic-apps"></a>Azure Logic Apps'ten HTTP veya HTTPS üzerinden hizmet uç noktalarını çağırma

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik http tetikleyicisi veya EYLEMIYLE, http veya https üzerinden hizmet uç noktalarına istek gönderen otomatikleştirilmiş görevler ve iş akışları oluşturabilirsiniz. Örneğin, bu uç noktayı belirli bir zamanlamaya göre denetleyerek Web siteniz için hizmet uç noktasını izleyebilirsiniz. Bu uç noktada belirtilen olay, Web siteniz gibi olduğunda, olay mantıksal uygulamanızın iş akışını tetikler ve bu iş akışındaki eylemleri çalıştırır. Bunun yerine gelen HTTPS çağrılarını alıp yanıtlamak istiyorsanız, yerleşik [istek tetikleyicisi veya Yanıt eylemini](../connectors/connectors-native-reqres.md)kullanın.

* Bir uç noktayı yinelenen bir zamanlamaya göre denetlemek veya *yoklamak* IÇIN, [http tetikleyicisini](#http-trigger) iş akışınıza ilk adım olarak ekleyin. Tetikleyici uç noktayı her denetlediğinde tetikleyici, uç noktaya bir istek çağırır veya bir *istek* gönderir. Uç noktanın yanıtı, mantıksal uygulamanızın iş akışının çalışıp çalışmadığını belirler. Tetikleyici, mantıksal uygulamanızdaki eylemlere bitiş noktası yanıtından içerik geçirir.

* Bir uç noktayı iş akışınızda başka herhangi bir yerde çağırmak için [http eylemini ekleyin](#http-action). Uç noktanın yanıtı, iş akışınızın kalan eylemlerinin nasıl çalışacağını belirler.

Bu makalede mantıksal uygulamanızın iş akışına bir HTTP tetikleyicisi veya eyleminin nasıl ekleneceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Çağırmak istediğiniz hedef uç noktanın URL 'SI

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)hakkında temel bilgi. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md)olduğunu gözden geçirin.

* Hedef uç noktayı çağırmak istediğiniz mantıksal uygulama. HTTP tetikleyicisiyle başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). HTTP eylemini kullanmak için mantıksal uygulamanızı istediğiniz tetikleyiciyle başlatın. Bu örnek, ilk adım olarak HTTP tetikleyicisini kullanır.

<a name="http-trigger"></a>

## <a name="add-an-http-trigger"></a>HTTP tetikleyicisi ekleme

Bu yerleşik tetikleyici, bir uç nokta için belirtilen URL 'ye HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure portalında](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında boş mantıksal uygulamanızı açın.

1. Tasarımcı 'nın arama kutusunda **yerleşik**' i seçin. Arama kutusuna `http` filtreniz olarak yazın. **Tetikleyiciler** listesinden **http** tetikleyicisi ' ni seçin.

   ![HTTP tetikleyicisi'ni seçin](./media/connectors-native-http/select-http-trigger.png)

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

<a name="tls-support"></a>

## <a name="transport-layer-security-tls"></a>Aktarım Katmanı Güvenliği (TLS)

Hedef uç noktanın özelliği temel alınarak, giden çağrılar daha önce Güvenli Yuva Katmanı (SSL), sürüm 1,0, 1,1 ve 1,2 olan Aktarım Katmanı Güvenliği 'ni (TLS) destekler. Logic Apps, uç nokta ile mümkün olan en yüksek desteklenen sürümü kullanarak görüşür.

Örneğin, uç nokta 1,2 destekliyorsa, HTTP Bağlayıcısı önce 1,2 ' i kullanır. Aksi halde, bağlayıcı desteklenen bir sonraki en yüksek sürümü kullanır.

<a name="self-signed"></a>

## <a name="self-signed-certificates"></a>Otomatik olarak imzalanan sertifikalar

* Genel, çok kiracılı Azure ortamındaki Logic Apps için, HTTP Bağlayıcısı kendinden imzalı TLS/SSL sertifikalarına izin vermez. Mantıksal uygulamanız sunucuya HTTP çağrısı yapar ve bir TLS/SSL otomatik olarak imzalanan sertifika sunduğunda, HTTP çağrısı hata vererek başarısız olur `TrustFailure` .

* Bir [tümleştirme hizmeti ortamındaki (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)Logic Apps için http BAĞLAYıCıSı, TLS/SSL el sıkışmaları için otomatik olarak imzalanan sertifikalara izin verir. Ancak, önce Logic Apps REST API kullanarak mevcut bir ıSE veya yeni bir ıSE için [otomatik olarak imzalanan sertifika desteğini etkinleştirmeniz](../logic-apps/create-integration-service-environment-rest-api.md#request-body) ve ortak sertifikayı konuma yüklemeniz gerekir `TrustedRoot` .

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

<a name="asynchronous-pattern"></a>

## <a name="asynchronous-request-response-behavior"></a>Zaman uyumsuz istek-yanıt davranışı

Varsayılan olarak, Azure Logic Apps içindeki tüm HTTP tabanlı eylemler standart [zaman uyumsuz işlem modelini](/azure/architecture/patterns/async-request-reply)izler. Bu model bir HTTP eylemi bir uç nokta, hizmet, sistem veya API 'ye bir istek çağırdıktan sonra, alıcı hemen ["202 kabul edildi"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.3) yanıtı döndürdüğünü belirtir. Bu kod, alıcının isteği kabul ettiğini ancak işlemeyi bitirmediğini onaylar. Yanıt, `location` çağıran işlemeyi durdurmadan ve ["200 Tamam"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.1) başarı yanıtını ya da 202 olmayan diğer yanıtı döndürünceye kadar, çağıranın zaman uyumsuz isteğin durumunu yoklamak veya denetlemek için kullanabileceği, URL 'YI ve yenileme kimliğini belirten bir üst bilgi içerebilir. Ancak çağıranın işlemin tamamlanmasını beklemek zorunda değildir ve sonraki eylemi çalıştırmaya devam edebilir. Daha fazla bilgi için bkz. [zaman uyumsuz mikro hizmet tümleştirmesi mikro hizmet bağımsız çalışma sınırı zorlar](/azure/architecture/microservices/design/interservice-communication#synchronous-versus-asynchronous-messaging).

* Mantıksal uygulama tasarımcısında, HTTP eyleminin, ancak tetiklemenin, varsayılan olarak etkinleştirilen bir **zaman uyumsuz model** ayarı vardır. Bu ayar, çağıranın işlemin bitmesini beketmediğini ve bir sonraki eyleme geçebilir ancak işlem duraklarına kadar durumu denetlemeye devam edebilir. Devre dışı bırakılırsa, bu ayar çağıranın bir sonraki eyleme geçmeden önce işlemin tamamlanmasını beklediğini belirtir.

  Bu ayarı bulmak için şu adımları izleyin:

  1. HTTP eyleminin başlık çubuğunda, eylemin ayarlarını açan üç nokta (**...**) düğmesini seçin.

  1. **Zaman uyumsuz model** ayarını bulun.

     !["Zaman uyumsuz model" ayarı](./media/connectors-native-http/asynchronous-pattern-setting.png)

* HTTP eyleminin temel alınan JavaScript Nesne Gösterimi (JSON) tanımı, zaman uyumsuz işlem modelini örtülü olarak izler.

<a name="disable-asynchronous-operations"></a>

## <a name="disable-asynchronous-operations"></a>Zaman uyumsuz işlemleri devre dışı bırak

Bazen, örneğin, aşağıdakileri yapmak istediğinizde, belirli senaryolarda HTTP eyleminin zaman uyumsuz davranışına isteyebilirsiniz:

* [Uzun süre çalışan görevler için HTTP zaman aşımlarını önleyin](#avoid-http-timeouts)
* [Konum üst bilgilerini denetlemeyi devre dışı bırak](#disable-location-header-check)

<a name="turn-off-asynchronous-pattern-setting"></a>

### <a name="turn-off-asynchronous-pattern-setting"></a>**Zaman uyumsuz model** ayarını kapat

1. Mantıksal uygulama Tasarımcısı ' nda, HTTP eyleminin başlık çubuğunda, eylemin ayarlarını açan üç nokta (**...**) düğmesini seçin.

1. **Zaman uyumsuz model** ayarını bulun, etkinleştirilirse ayarı **devre dışı** bırakın ve **bitti**' yi seçin.

   !["Zaman uyumsuz model" ayarını devre dışı bırak](./media/connectors-native-http/disable-asynchronous-pattern-setting.png)

<a name="add-disable-async-pattern-option"></a>

### <a name="disable-asynchronous-pattern-in-actions-json-definition"></a>Eylemin JSON tanımında zaman uyumsuz kalıbı devre dışı bırak

HTTP eyleminin temel alınan JSON tanımında [ `"DisableAsyncPattern"` işlem seçeneğini](../logic-apps/logic-apps-workflow-actions-triggers.md#operation-options) eylemin tanımına ekleyerek eylemin yerine eşzamanlı işlem modelini takip edin. Daha fazla bilgi için bkz. Ayrıca, [zaman uyumlu bir işlem modelinde eylemleri çalıştırma](../logic-apps/logic-apps-workflow-actions-triggers.md#disable-asynchronous-pattern).

<a name="avoid-http-timeouts"></a>

## <a name="avoid-http-timeouts-for-long-running-tasks"></a>Uzun süre çalışan görevler için HTTP zaman aşımlarını önleyin

HTTP isteklerinin [zaman aşımı sınırı](../logic-apps/logic-apps-limits-and-config.md#http-limits)vardır. Bu sınır nedeniyle zaman aşımına uğramadan uzun süre çalışan bir HTTP eyleminiz varsa, şu seçeneklere sahip olursunuz:

* Eylemin sürekli olarak yoklama yapması veya isteğin durumunu denetlemesi için [http eyleminin zaman uyumsuz işlem modelini devre dışı bırakın](#disable-asynchronous-operations) . Bunun yerine, eylem, isteğin işlemeyi bitirdikten sonra alıcının durum ve sonuçlarla yanıt vermesini bekler.

* HTTP eylemini, isteğin işlemeyi bitirdikten sonra alıcının durum ve sonuçlarla yanıt vermesini bekleyen [http Web kancası eylemiyle](../connectors/connectors-native-webhook.md)değiştirin.

<a name="disable-location-header-check"></a>

## <a name="disable-checking-location-headers"></a>Konum üst bilgilerini denetlemeyi devre dışı bırak

Bazı uç noktalar, hizmetler, sistemler veya API 'Ler, üst bilgisi olmayan bir "202 kabul EDILDI" yanıtı döndürür `location` . Üst bilgi mevcut olmadığında HTTP eyleminin sürekli olarak istek durumunu denetlemesini önlemek için `location` şu seçeneklere sahip olabilirsiniz:

* Eylemin sürekli olarak yoklama yapması veya isteğin durumunu denetlemesi için [http eyleminin zaman uyumsuz işlem modelini devre dışı bırakın](#disable-asynchronous-operations) . Bunun yerine, eylem, isteğin işlemeyi bitirdikten sonra alıcının durum ve sonuçlarla yanıt vermesini bekler.

* HTTP eylemini, isteğin işlemeyi bitirdikten sonra alıcının durum ve sonuçlarla yanıt vermesini bekleyen [http Web kancası eylemiyle](../connectors/connectors-native-webhook.md)değiştirin.

## <a name="known-issues"></a>Bilinen sorunlar

<a name="omitted-headers"></a>

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

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyici ve eylem parametreleri hakkında daha fazla bilgi için aşağıdaki bölümlere bakın:

* [HTTP tetikleyici parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger)
* [HTTP eylem parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)

### <a name="output-details"></a>Çıkış ayrıntıları

Bu bilgileri döndüren bir HTTP tetikleyicisinden veya eylemden çıktılar hakkında daha fazla bilgi bulabilirsiniz:

| Özellik | Tür | Açıklama |
|----------|------|-------------|
| `headers` | JSON nesnesi | İstekten gelen üstbilgiler |
| `body` | JSON nesnesi | İstekten gelen gövde içeriğine sahip nesne |
| `status code` | Tamsayı | İstekteki durum kodu |
|||

| Durum kodu | Açıklama |
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

