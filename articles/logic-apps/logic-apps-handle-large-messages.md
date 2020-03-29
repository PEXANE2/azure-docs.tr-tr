---
title: Büyük iletileri yığın kullanarak işleme
description: Azure Logic Apps ile oluşturduğunuz otomatik görevlerde ve iş akışlarında yığın kullanarak büyük ileti boyutlarını nasıl işleyeceğinizi öğrenin
services: logic-apps
ms.suite: integration
author: shae-hurst
ms.author: shhurst
ms.topic: article
ms.date: 12/03/2019
ms.openlocfilehash: 81e7c12b04c1ebd9691c11d76f387f7d42490180
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75456560"
---
# <a name="handle-large-messages-with-chunking-in-azure-logic-apps"></a>Azure Mantıksal Uygulamaları'nda yığınla büyük iletileri işleme

İletileri işlerken, Logic Apps ileti içeriğini maksimum boyuta sınırlar. Bu sınır, büyük iletilerin depolanması ve işlenmesiyle oluşturulan ek yükün azaltılmasına yardımcı olur. Bu sınırdan daha büyük iletileri işlemek için, Logic Apps büyük bir iletiyi daha küçük iletilere *ayırabilir.* Bu şekilde, belirli koşullar altında Logic Apps'ı kullanarak büyük dosyaları aktarabilirsiniz. Diğer hizmetlerle bağlayıcılar veya HTTP aracılığıyla iletişim kurarken, Logic Apps büyük iletileri *yalnızca* parçalar halinde tüketebilir. Bu durum, bağlayıcıların da ötme yi desteklemesi veya Mantık Uygulamaları ile bu hizmetler arasındaki temel HTTP ileti alışverişinin yığınlama yı kullanması gerektiği anlamına gelir.

Bu makalede, sınırdan daha büyük iletileri işleme eylemleri için yığın ayarlama nasıl gösterilmektedir. Logic App tetikleyicileri, birden çok ileti alışverişinin artan yükü nedeniyle yığınlamayı desteklemez. 

## <a name="what-makes-messages-large"></a>İletileri "büyük" yapan nedir?

İletiler, bu iletileri işleyen hizmete göre "büyüktür". Büyük iletiler üzerindeki tam boyut sınırı Logic Apps ve bağlayıcılar arasında farklılık gösterir. Hem Logic Apps hem de bağlayıcılar, parçalanmış olması gereken büyük iletileri doğrudan tüketemez. Logic Apps ileti boyutu sınırı [için, Logic Apps sınırları ve yapılandırmabakın.](../logic-apps/logic-apps-limits-and-config.md)
Her bağlayıcının ileti boyutu sınırı için [bağlayıcının özel teknik ayrıntılarına](../connectors/apis-list.md)bakın.

### <a name="chunked-message-handling-for-logic-apps"></a>Logic Apps için yığınlı ileti işleme

Logic Apps, ileti boyutu sınırından daha büyük yığınlı iletilerden çıkan çıktıları doğrudan kullanamaz. Yalnızca ötmeyi destekleyen eylemler bu çıktılardaki ileti içeriğine erişebilir. Bu nedenle, büyük iletileri işleyen bir eylem aşağıdaki *ölçütleri karşılamalıdır:*

* Bu eylem bir bağlayıcıya ait olduğunda yerel olarak yığınlamayı destekler. 
* Bu eylemin çalışma zamanı yapılandırmasında yığın desteği etkinleştirin. 

Aksi takdirde, büyük içerik çıktısına erişmeye çalıştığınızda çalışma zamanı hatası alırsınız. Ösyerek etkinleştirmek için [bkz.](#set-up-chunking)

### <a name="chunked-message-handling-for-connectors"></a>Bağlayıcılar için yığınlı ileti işleme

Logic Apps ile iletişim yapan hizmetlerin kendi ileti boyutu sınırları olabilir. Bu sınırlar genellikle Logic Apps sınırından daha küçüktür. Örneğin, bir bağlayıcının ötmeyi desteklediğini varsayarsak, bağlayıcı 30 MB'lık bir iletiyi büyük olarak kabul edebilirken, Logic Apps bunu yapmaz. Bu bağlayıcının sınırına uymak için, Logic Apps 30 MB'dan büyük iletileri daha küçük parçalara böler.

Östeneleri destekleyen bağlayıcılar için, temel öbeklenme protokolü son kullanıcılar tarafından görünmez. Ancak, tüm bağlayıcılar ötme'yi desteklemez, bu nedenle gelen iletiler bağlayıcıların boyut sınırlarını aştığında bu bağlayıcılar çalışma zamanı hataları oluşturur.

> [!NOTE]
> Yığın lama kullanan eylemler için tetikleyici gövdeyi geçemez veya bu `@triggerBody()?['Content']` eylemlerdeki gibi ifadeleri kullanamazsınız. Bunun yerine, metin veya JSON dosya içeriği [ **için, Eylem Oluştur'u** ](../logic-apps/logic-apps-perform-data-operations.md#compose-action) kullanmayı deneyebilir veya bu içeriği işlemek için [bir değişken oluşturabilirsiniz.](../logic-apps/logic-apps-create-variables-store-values.md) Tetikleyici gövde, ortam dosyaları gibi diğer içerik türleri içeriyorsa, bu içeriği işlemek için başka adımlar gerçekleştirmeniz gerekir.

<a name="set-up-chunking"></a>

## <a name="set-up-chunking-over-http"></a>HTTP üzerinde yığın ayarlama

Genel HTTP senaryolarında, mantık uygulamanızın ve bitiş noktanızın büyük iletialışverişinde bulunabilmesi için büyük içerik indirmelerini ve yüklemelerini HTTP üzerinden bölebilirsiniz. Ancak, iletileri Logic Apps'ın beklediği şekilde ötmelisiniz. 

Bir bitiş noktası indirmeler veya yüklemeler için ötme özelliği etkinleştirdiyse, mantık uygulamanızdaki HTTP eylemleri büyük iletileri otomatik olarak öper. Aksi takdirde, bitiş noktasında yığın desteği ayarlamanız gerekir. Bitiş noktasının veya bağlayıcının sahibi değil seniz veya denetiminiz yoksa, yığınlama ayarlama seçeneğiniz olmayabilir.

Ayrıca, bir HTTP eylemi zaten ötme özelliğini etkinleştirmiyorsa, eylemin `runTimeConfiguration` özelliğinde yığın ayarlamanız gerekir. Bu özelliği, doğrudan daha sonra açıklandığı şekilde kod görünümü düzenleyicisinde veya burada açıklandığı gibi Logic Apps Tasarımcısı'nda eylem içinde ayarlayabilirsiniz:

1. HTTP eyleminin sağ üst köşesinde, elips düğmesini seçin (**...**), ve ardından **Ayarlar'ı**seçin.

   ![Eylemde ayarlar menüsünü açın](./media/logic-apps-handle-large-messages/http-settings.png)

2. **İçerik Aktarımı**altında, **"İzin Ver"i** **A'ya**ayarlayın.

   ![Yığınlamayı açma](./media/logic-apps-handle-large-messages/set-up-chunking.png)

3. İndirme ler veya yüklemeler için yığın ayarlamaya devam etmek için aşağıdaki bölümlerle devam edin.

<a name="download-chunks"></a>

## <a name="download-content-in-chunks"></a>İçeriği parçalar halinde indirin

Birçok uç nokta, http GET isteği aracılığıyla indirildiğinde otomatik olarak büyük iletileri parçalar halinde gönderir. Öbeklenmiş iletileri HTTP üzerinden bir bitiş noktasından indirmek için, bitiş noktasının kısmi içerik isteklerini veya *yığınla indirilenleri*desteklemesi gerekir. Mantık uygulamanız içeriği indirmek için bir bitiş noktasına BIR HTTP GET isteği gönderdiğinde ve bitiş noktası "206" durum koduyla yanıt verdiğinde, yanıt tıkanmış içerik içerir. Logic Apps, bitiş noktasının kısmi istekleri destekleyip desteklemediğini denetemez. Ancak, mantık uygulamanız ilk "206" yanıtını aldığında, mantık uygulamanız tüm içeriği indirmek için otomatik olarak birden çok istek gönderir.

Bir bitiş noktasının kısmi içeriği destekleyip desteklemeyeceğini kontrol etmek için HEAD isteği gönderin. Bu istek, yanıtın `Accept-Ranges` üstbilgi içinde olup olmadığını belirlemenize yardım eder. Bu şekilde, bitiş noktası ödenmiş indirmeleri destekliyor sa da ödenmiyorsa, `Range` http GET isteğinizdeki üstbilgiyi ayarlayarak bu seçeneği *önerebilirsiniz.* 

Bu adımlar, Logic Apps'ın tıklım tıklım daki içeriği bir bitiş noktasından mantık uygulamanıza indirmek için kullandığı ayrıntılı işlemi açıklar:

1. Mantık uygulamanız bitiş noktasına bir HTTP GET isteği gönderir.

   İstek üstbilgisi isteğe `Range` bağlı olarak içerik parçaları istemek için bir bayt aralığını açıklayan bir alan içerebilir.

2. Bitiş noktası "206" durum kodu ve bir HTTP ileti gövdesi ile yanıt verir.

    Bu yığındaki içerikle ilgili ayrıntılar, `Content-Range` Logic Apps'ın öbek için başlangıç ve bitişi belirlemelerine yardımcı olan bilgiler ve parçalamadan önce tüm içeriğin toplam boyutunu da içeren yanıtın üstbilgisinde görünür.

3. Mantık uygulamanız otomatik olarak takip http GET isteklerini gönderir.

    Mantık uygulamanız, tüm içerik alınana kadar takip GET istekleri gönderir.

Örneğin, bu eylem tanımı, `Range` üstbilgi ayarlayan bir HTTP GET isteği gösterir. Üstbilgi, bitiş noktasının tıklım tıklım içerikle yanıt vermesi gerektiğini *önerir:*

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

GET isteği "Aralık" üstbilgisini bayt aralığı olan "bayt=0-1023" olarak ayarlar. Bitiş noktası kısmi içerik isteklerini destekliyorsa, bitiş noktası istenen aralıktan bir içerik parçasıyla yanıt verir. Bitiş noktasına bağlı olarak, "Aralık" üstbilgi alanının tam biçimi farklı olabilir.

<a name="upload-chunks"></a>

## <a name="upload-content-in-chunks"></a>İçeriği parçalar halinde yükleme

Bir HTTP eyleminden yığınlı içerik yüklemek için, eylemin eylemin `runtimeConfiguration` özelliği üzerinden yığın desteğietkin leştirilmiş olması gerekir. Bu ayar, eylemin yığınlama protokolünü başlatmasına izin verir. Mantık uygulamanız daha sonra hedef bitiş noktasına bir ilk POST veya PUT iletisi gönderebilir. Bitiş noktası önerilen bir yığın boyutuyla yanıt verdikten sonra, mantık uygulamanız içerik parçalarını içeren HTTP PATCH isteklerini göndererek takip eder.

Bu adımlar, Logic Apps'ın mantıksal uygulamanızdan bir bitiş noktasına yığınlı içeriği yüklemek için kullandığı ayrıntılı işlemi açıklar:

1. Mantık uygulamanız boş bir ileti gövdesi ile ilk HTTP POST veya PUT isteği gönderir. İstek üstbilgisi, mantık uygulamanızın yığınlar halinde yüklemek istediği içerikle ilgili şu bilgileri içerir:

   | Logic Apps üstbilgi alanı isteme | Değer | Tür | Açıklama |
   |---------------------------------|-------|------|-------------|
   | **x-ms-transfer modu** | Yığın | Dize | İçeriğin parçalar halinde yüklendiğini gösterir |
   | **x-ms-içerik uzunluğu** | <*içerik uzunluğu*> | Tamsayı | Parçalamadan önce baytlarda tüm içerik boyutu |
   ||||

2. Bitiş noktası "200" başarı durum kodu ve bu isteğe bağlı bilgilerle yanıt verir:

   | Uç nokta yanıt üstbilgi alanı | Tür | Gerekli | Açıklama |
   |--------------------------------|------|----------|-------------|
   | **x-ms-ösyğboyutu** | Tamsayı | Hayır | Baytlarda önerilen parça boyutu |
   | **Konum** | Dize | Evet | HTTP PATCH iletilerinin gönderilen URL konumu |
   ||||

3. Mantık uygulamanız her biri bu bilgileri içeren http PATCH iletileri oluşturur ve gönderir:

   * **X-ms-içerik uzunluğuna** tekabün toplamı olan tüm içerik sırayla yüklenene kadar **x-ms-chunk boyutuna** veya dahili olarak hesaplanan bir boyuta dayalı bir içerik yığını

   * Her PATCH iletisinde gönderilen içerik öbek hakkında bu üstbilgi ayrıntıları:

     | Logic Apps üstbilgi alanı isteme | Değer | Tür | Açıklama |
     |---------------------------------|-------|------|-------------|
     | **İçerik Aralığı** | <*Aralığı*> | Dize | Başlangıç değeri, bitiş değeri ve toplam içerik boyutu da dahil olmak üzere geçerli içerik yığınının bayt aralığı, örneğin: "bayt=0-1023/10100" |
     | **İçerik Türü** | <*içerik türü*> | Dize | Parçalanan içeriğin türü |
     | **İçerik Uzunluğu** | <*içerik uzunluğu*> | Dize | Geçerli parçanın baytlarındaki boyutun uzunluğu |
     |||||

4. Her PATCH isteğinden sonra, bitiş noktası "200" durum kodu ve aşağıdaki yanıt üstbilgileri yle yanıt vererek her bir öbek için makbuzonaylar:

   | Uç nokta yanıt üstbilgi alanı | Tür | Gerekli | Açıklama |
   |--------------------------------|------|----------|-------------|
   | **Aralığı** | Dize | Evet | Uç noktaya kadar alınan içerik için bayt aralığı, örneğin: "bayt=0-1023" |   
   | **x-ms-ösyğboyutu** | Tamsayı | Hayır | Baytlarda önerilen parça boyutu |
   ||||

Örneğin, bu eylem tanımı, ödenerek içeriği bitiş noktasına yüklemek için bir HTTP POST isteğini gösterir. Eylemin `runTimeConfiguration` özelliğinde, `contentTransfer` özellik aşağıdakileri `transferMode` `chunked`ayarlar:

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
