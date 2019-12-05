---
title: Büyük iletileri işleme
description: Azure Logic Apps öbek ile büyük ileti boyutlarını nasıl işleyeceğinizi öğrenin
services: logic-apps
ms.suite: integration
author: shae-hurst
ms.author: shhurst
ms.topic: article
ms.date: 12/03/2019
ms.openlocfilehash: 8c2e857808b0638fbba54cfe9a623ba3fd764119
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74815091"
---
# <a name="handle-large-messages-with-chunking-in-azure-logic-apps"></a>Azure Logic Apps parçalama ile büyük iletileri işleme

İletileri işlerken Logic Apps ileti içeriğini en büyük boyuta sınırlandırır. Bu sınır, büyük iletileri depolayarak ve işleyerek oluşturulan ek yükün azaltılmasına yardımcı olur. Bu sınırdan daha büyük iletileri işlemek için Logic Apps, büyük bir iletiyi daha küçük iletilere *öbekedebilir* . Bu şekilde, belirli koşullar altında Logic Apps kullanarak büyük dosyaları aktarmaya devam edebilirsiniz. Bağlayıcılar veya HTTP üzerinden diğer hizmetlerle iletişim kurarken Logic Apps, *yalnızca* parçalar halinde büyük iletileri kullanabilir. Bu durum, bağlayıcıların öbek oluşturma 'yı desteklemesi veya Logic Apps ile bu hizmetler arasında temel alınan HTTP ileti alışverişi için parçalama kullanması gerekir.

Bu makalede, sınırdan daha büyük iletileri işleyen eylemler için nasıl öbek ayarlayabileceğiniz gösterilmektedir. Mantıksal uygulama Tetikleyicileri, birden fazla iletiyi değiş tokuş eden daha fazla ek yük nedeniyle parçalama desteği vermez. 

## <a name="what-makes-messages-large"></a>"Büyük" iletileri ne yapar?

İletiler, bu iletileri işleyen hizmete göre "büyük". Büyük iletilerde tam boyut sınırı Logic Apps ve bağlayıcılar arasında farklılık gösterir. Hem Logic Apps hem de bağlayıcılar, öbekli olması gereken büyük iletileri doğrudan tüketmez. Logic Apps ileti boyutu sınırı için bkz. [Logic Apps sınırları ve yapılandırması](../logic-apps/logic-apps-limits-and-config.md).
Her bağlayıcının ileti boyutu sınırı için, [bağlayıcının ilgili teknik ayrıntılarına](../connectors/apis-list.md)bakın.

### <a name="chunked-message-handling-for-logic-apps"></a>Logic Apps için öbekli ileti işleme

Logic Apps, ileti boyutu sınırından daha büyük olan öbekli iletilerden gelen çıktıları doğrudan kullanamaz. Yalnızca parçalama desteği olan eylemler, bu çıkışlara ait ileti içeriğine erişebilir. Bu nedenle, büyük iletileri işleyen bir *eylemin şu ölçütlere* uyması gerekir:

* Bu eylem bir bağlayıcıya ait olduğunda yerel olarak parçalama desteği. 
* Bu eylemin çalışma zamanı yapılandırmasında öbek oluşturma desteği etkin. 

Aksi takdirde, büyük içerik çıktısına erişmeye çalıştığınızda bir çalışma zamanı hatası alırsınız. Parçalama özelliğini etkinleştirmek için bkz. [Parçalama desteğini ayarlama](#set-up-chunking).

### <a name="chunked-message-handling-for-connectors"></a>Bağlayıcılar için öbekli ileti işleme

Logic Apps ile iletişim kuran hizmetler kendi ileti boyutu sınırlarına sahip olabilir. Bu sınırlar genellikle Logic Apps sınırından daha küçüktür. Örneğin, bir bağlayıcının parçalama desteklediğini varsayarsak, bağlayıcı 30 MB 'lık bir iletiyi büyük olarak kabul edebilir, Logic Apps. Bu bağlayıcının sınırına uymak için, Logic Apps 30 MB 'tan daha büyük bir iletiyi daha küçük parçalara böler.

Parçalama desteği sunan bağlayıcılar için, temel alınan Parçalama Protokolü son kullanıcılara görünmez. Ancak, tüm bağlayıcılar öbek oluşturmayı desteklemez, bu nedenle bu bağlayıcılar, gelen iletiler bağlayıcıların boyut sınırlarını aştığında çalışma zamanı hataları oluşturur.

> [!NOTE]
> Öbek kullanan eylemler için, tetikleyici gövdesini geçiremiyor veya bu eylemlerde `@triggerBody()?['Content']` gibi ifadeler kullanamıyoruz. Bunun yerine, metin veya JSON dosya içeriği için, [ **oluşturma** eylemini](../logic-apps/logic-apps-perform-data-operations.md#compose-action) kullanmayı deneyebilir veya bu içeriği işlemek için [bir değişken oluşturabilirsiniz](../logic-apps/logic-apps-create-variables-store-values.md) . Tetikleyici gövdesi, medya dosyaları gibi diğer içerik türlerini içeriyorsa, bu içeriği işlemek için diğer adımları gerçekleştirmeniz gerekir.

<a name="set-up-chunking"></a>

## <a name="set-up-chunking-over-http"></a>HTTP üzerinden parçalama ayarlama

Genel HTTP senaryolarında, mantıksal uygulamanızın ve bir uç noktanın büyük iletileri alışverişi yapabilmesi için HTTP üzerinden büyük içerik indirmeleri ve karşıya yüklemeleri bölebilirsiniz. Ancak, iletilerin Logic Apps beklediği şekilde öbek iletileri olması gerekir. 

Bir uç nokta indirme veya karşıya yükleme için parçalama etkinleştirmişse, mantıksal uygulamanızdaki HTTP eylemleri otomatik olarak büyük iletileri öbektir. Aksi takdirde, uç noktada parçalama desteği ayarlamanız gerekir. Uç nokta veya bağlayıcınızı bilmiyorsanız veya denetmezseniz, parçalama ayarlama seçeneğiniz olmayabilir.

Ayrıca, bir HTTP eylemi zaten parçalama etkinleştirmezse, eylemin `runTimeConfiguration` özelliğinde de parçalama ayarlamanız gerekir. Bu özelliği, doğrudan kod görünümü düzenleyicisinde, daha sonra açıklandığı şekilde veya Logic Apps tasarımcısında, burada açıklandığı gibi, eylem içinde ayarlayabilirsiniz:

1. HTTP eyleminin sağ üst köşesinde üç nokta düğmesini ( **...** ) ve ardından **Ayarlar**' ı seçin.

   ![Eylemde, Ayarlar menüsünü açın](./media/logic-apps-handle-large-messages/http-settings.png)

2. **Içerik aktarımı**altında, **öbek kümesine izin ver** ' i **Açık**olarak ayarlayın.

   ![Parçalama 'yi aç](./media/logic-apps-handle-large-messages/set-up-chunking.png)

3. Yüklemeler veya karşıya yüklemeler için parçalama ayarlamaya devam etmek için aşağıdaki bölümlerle devam edin.

<a name="download-chunks"></a>

## <a name="download-content-in-chunks"></a>Öbeklerdeki içeriği indirme

Bir HTTP GET isteği aracılığıyla indirilen birçok uç nokta, öbeklere otomatik olarak büyük iletiler gönderir. HTTP üzerinden bir uç noktadan öbekli iletileri indirmek için, uç noktanın kısmi içerik isteklerini veya *öbekli İndirmeleri*desteklemesi gerekir. Mantıksal uygulamanız içerik indirmek için bir uç noktaya HTTP GET isteği gönderdiğinde ve uç nokta bir "206" durum kodu ile yanıt verirse, yanıt öbekli içerik içerir. Logic Apps, bir uç noktanın kısmi istekleri destekleyip desteklemediğini denetleyemiyorum. Ancak, mantıksal uygulamanız ilk "206" yanıtını aldığında, mantıksal uygulamanız tüm içeriği indirmek için otomatik olarak birden çok istek gönderir.

Bir uç noktanın kısmi içeriği destekleyemeyeceğini denetlemek için bir HEAD isteği gönderin. Bu istek, yanıtın `Accept-Ranges` üst bilgisini içerip içermediğini belirlemenize yardımcı olur. Bu şekilde, uç nokta, öbekli İndirmeleri destekliyorsa ancak öbekli içerik göndermezse, HTTP GET talebinizdeki `Range` üst bilgisini ayarlayarak bu seçeneği *önerebilirsiniz* . 

Bu adımlar, bir uç noktadan mantıksal uygulamanıza öbekli içerik indirmek için kullanılan Logic Apps ayrıntılı süreci anlatmaktadır:

1. Mantıksal uygulamanız uç noktaya bir HTTP GET isteği gönderir.

   İstek üst bilgisi isteğe bağlı olarak, içerik öbekleri istemek için bir bayt aralığını açıklayan bir `Range` alanı içerebilir.

2. Uç nokta, "206" durum kodu ve bir HTTP ileti gövdesi ile yanıt verir.

    Bu öbekteki içerik hakkındaki ayrıntılar yanıtın `Content-Range` üst bilgisinde görünür ve bu, öbek için başlangıç ve bitiş bilgilerini ve parçalama öncesinde tüm içeriğin toplam boyutunu belirlemenize Logic Apps yardımcı olan bilgiler de dahildir.

3. Mantıksal uygulamanız, izleme HTTP GET isteklerini otomatik olarak gönderir.

    Mantıksal uygulamanız, tüm içerik alınana kadar izleme Al istekleri gönderir.

Örneğin, bu eylem tanımı `Range` üst bilgisini ayarlayan bir HTTP GET isteği gösterir. Üstbilgi, uç noktanın öbekli içerikle yanıt vermesini *önerir* :

```json
"getAction": {
    "inputs": {
        "headers": {
            "Range": "bytes=0-1023"
        },
       "method": "GET",
       "uri": "http://myAPIendpoint/api/downloadContent"
    },
    "runAfter": {},
    "type": "Http"
}
```

GET isteği, "Range" üst bilgisini "byte = 0-1023" olarak ayarlar ve bu da bayt aralığıdır. Uç nokta, kısmi içerik isteklerini destekliyorsa, uç nokta istenen aralıktaki bir içerik öbeğiyle yanıt verir. Uç nokta temelinde, "Aralık" başlık alanı için tam biçim farklı olabilir.

<a name="upload-chunks"></a>

## <a name="upload-content-in-chunks"></a>Öbeklere içerik yükleme

Bir HTTP eyleminden öbekli içerik yüklemek için eylemin `runtimeConfiguration` özelliği aracılığıyla öbek desteğini etkinleştirmiş olması gerekir. Bu ayar eyleme öbek protokolünü başlatma izni verir. Mantıksal uygulamanız daha sonra bir ilk GÖNDERI gönderebilir veya hedef uç noktaya ileti YERLEŞTIREBILIR. Uç nokta, önerilen bir öbek boyutuyla yanıt verdikten sonra mantıksal uygulamanız, içerik öbeklerini içeren HTTP PATCH istekleri göndererek takip eder.

Bu adımlarda, mantıksal uygulamanızdan bir uç noktaya öbekli içerik yüklemek için kullanılan ayrıntılı işlem Logic Apps açıklanmıştır:

1. Mantıksal uygulamanız, boş bir ileti gövdesine sahip bir ilk HTTP POST veya PUT isteği gönderir. İstek üst bilgisi, mantıksal uygulamanızın parçalara yüklemek istediği içerikle ilgili şu bilgileri içerir:

   | Logic Apps istek üst bilgisi alanı | Değer | Tür | Açıklama |
   |---------------------------------|-------|------|-------------|
   | **x-ms-transfer-mode** | öbekli | Dize | İçeriğin öbeklerde karşıya yüklendiğini belirtir |
   | **x-MS-Content-Length** | <*Content-length*> | Tamsayı | Parçalama öncesinde tüm içerik boyutu bayt cinsinden |
   ||||

2. Uç nokta, "200" başarı durum kodu ve bu isteğe bağlı bilgiler ile yanıt verir:

   | Uç nokta yanıt üst bilgisi alanı | Tür | Gereklidir | Açıklama |
   |--------------------------------|------|----------|-------------|
   | **x-MS-öbek boyutu** | Tamsayı | Hayır | Önerilen öbek boyutu (bayt) |
   | **Konum** | Dize | Yes | HTTP PATCH iletilerinin gönderileceği URL konumu |
   ||||

3. Mantıksal uygulamanız, bu bilgileri içeren, izleme HTTP PATCH iletileri oluşturur ve gönderir:

   * X-ms- **Content-Length** tüm içerik toplama işlemi ardışık olarak karşıya yüklenene kadar **x-MS-öbek boyutunda** veya bazı dahili olarak hesaplanan boyut tabanlı bir içerik öbeği

   * Bu üst bilgi, her bir düzeltme eki iletisinde gönderilen içerik öbeği hakkındaki ayrıntıları:

     | Logic Apps istek üst bilgisi alanı | Değer | Tür | Açıklama |
     |---------------------------------|-------|------|-------------|
     | **Content-Range** | <*aralığı*> | Dize | Başlangıç değeri, bitiş değeri ve toplam içerik boyutu dahil olmak üzere geçerli içerik öbeği için bayt aralığı, örneğin: "bytes = 0-1023/10100" |
     | **Content-Type** | <*Content-type*> | Dize | Öbekli içerik türü |
     | **İçerik uzunluğu** | <*Content-length*> | Dize | Geçerli öbekteki bayt cinsinden boyut uzunluğu |
     |||||

4. Her bir düzeltme eki isteğinden sonra, uç nokta, "200" durum kodu ve aşağıdaki yanıt üst bilgileri ile yanıt vererek her bir öbek için alındı bilgisini onaylar:

   | Uç nokta yanıt üst bilgisi alanı | Tür | Gereklidir | Açıklama |
   |--------------------------------|------|----------|-------------|
   | **Aralık** | Dize | Yes | Uç nokta tarafından alınan içerik için bayt aralığı, örneğin: "bytes = 0-1023" |   
   | **x-MS-öbek boyutu** | Tamsayı | Hayır | Önerilen öbek boyutu (bayt) |
   ||||

Örneğin, bu eylem tanımı, bir uç noktaya öbekli içerik yüklemek için bir HTTP POST isteği gösterir. Eylemin `runTimeConfiguration` özelliğinde `contentTransfer` özelliği `chunked``transferMode` olarak ayarlanır:

```json
"postAction": {
    "runtimeConfiguration": {
        "contentTransfer": {
            "transferMode": "chunked"
        }
    },
    "inputs": {
        "method": "POST",
        "uri": "http://myAPIendpoint/api/action",
        "body": "@body('getAction')"
    },
    "runAfter": {
    "getAction": ["Succeeded"]
    },
    "type": "Http"
}
```
