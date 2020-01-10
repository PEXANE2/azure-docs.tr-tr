---
title: API biçimleri için kısıtlamalar ve Ayrıntılar desteği
titleSuffix: Azure API Management
description: Azure API Management 'de açık API, WSDL ve WADL biçimleri üzerinde bilinen sorunların ve kısıtlamaların ayrıntıları.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/02/2020
ms.author: apimpm
ms.openlocfilehash: a1c514368960d39834125bd497d05b3d9ebeae7c
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75640712"
---
# <a name="api-import-restrictions-and-known-issues"></a>API içeri aktarma kısıtlamaları ve bilinen sorunlar

## <a name="about-this-list"></a>Bu liste hakkında

Bir API 'yi içeri aktarırken, bazı kısıtlamalardan gelebilir veya içeri aktarma işlemini başarıyla gerçekleştirmeden önce düzeltilmesi gereken sorunları belirleyebilirsiniz. Bu makale, API 'nin içe aktarma biçimine göre düzenlenmiş bu sınırlamaları belgeler. Openapı dışa aktarmanın nasıl çalıştığını da açıklar.

## <a name="open-api"> </a>Openapı/Swagger içeri aktarma sınırlamaları

Openapı belgenizi içeri aktarırken hata alıyorsanız, önceden doğruladığınızdan emin olun. Tasarımcı 'yı Azure portal (Design-Front End-Openapı belirtim Editor) veya <a href="https://editor.swagger.io">Swagger Düzenleyicisi</a>gibi bir üçüncü taraf aracıyla kullanabilirsiniz.

### <a name="open-api-general"> </a>Genel

-   Hem yol hem de sorgu üzerinde gerekli parametreler benzersiz adlara sahip olmalıdır. (Openapı 'de bir parametre adının yalnızca bir konum içinde benzersiz olması gerekir, örneğin yol, sorgu, üstbilgi. Ancak API Management ' de, işlemlerin hem yol hem de sorgu parametrelerine göre ayırt edilebilir (Openapı 'Yi desteklemez). Bu nedenle, tüm URL şablonu içinde parametre adlarının benzersiz olması gerekir.)
-   `\$ref` işaretçileri dış dosyalara başvuramaz.
-   `x-ms-paths` ve `x-servers` yalnızca desteklenen uzantılardır.
-   Özel uzantılar İçeri aktarmada yok sayılır ve dışarı aktarma için kaydedilmez veya korunmaz.
-   `Recursion`-API Management, özyinelemeli olarak tanımlanan tanımları desteklemez (örneğin, kendilerine başvuran şemalar).
-   Kaynak dosya URL 'SI (varsa) göreli sunucu URL 'Lerine uygulanır.
-   Güvenlik tanımları yok sayılır.
-   API işlemleri için satır içi şema tanımları desteklenmez. Şema tanımları API kapsamında tanımlanır ve API işlemleri isteği veya Yanıt kapsamları içinde başvurulabilir.
-   Tanımlı URL parametresinin URL şablonunun bir parçası olması gerekir.
-   bir API tarafından döndürülen MIME türlerini açıklayan `Produces` anahtar sözcüğü desteklenmiyor. 

### <a name="open-api-v2"> </a>Openapı sürüm 2

-   Yalnızca JSON biçimi destekleniyor.

### <a name="open-api-v3"> </a>Openapı sürüm 3

-   Birçok `servers` belirtilirse, API Management ilk HTTPs URL 'sini seçmeyi dener. Herhangi bir HTTPs URL 'si yoksa, ilk HTTP URL 'si. HTTP URL 'Leri yoksa, sunucu URL 'SI boş olur.
-   `Examples` desteklenmez, ancak `example`.
-   `Multipart/form-data` desteklenmez.

## <a name="openapi-import-update-and-export-mechanisms"></a>Openapı içeri aktarma, güncelleştirme ve dışarı aktarma mekanizmaları

### <a name="add-new-api-via-openapi-import"></a>Openapı içeri aktarma aracılığıyla yeni API ekleme

Openapı belgesinde bulunan her bir işlem için, Azure Kaynak adı ve görünen ad ' ın sırasıyla `operationId` ve `summary` olarak ayarlandığı yeni bir işlem oluşturulur. `operationId` değer aşağıda açıklanan kuralların ardından normalleştirilir. `summary` değeri olduğu gibi içeri aktarılır ve uzunluğu 300 karakterle sınırlıdır.

`operationId` belirtilmemişse (yani, `null`veya boş değil), Azure Kaynak adı değeri HTTP yöntemi ve yol şablonu birleştirilerek (örneğin, `get-foo`) oluşturulur.

`summary` belirtilmemişse (yani, `null`veya boş), `display name` değeri `operationId`olarak ayarlanır. `operationId` belirtilmemişse, görünen ad değeri HTTP yöntemi ve yol şablonu birleştirilerek oluşturulur, örneğin, `Get - /foo`.

### <a name="update-an-existing-api-via-openapi-import"></a>Openapı içeri aktarma aracılığıyla var olan bir API 'YI güncelleştirme

İçeri aktarma sırasında mevcut API, Openapı belgesinde açıklanan API ile eşleşecek şekilde değiştirilir. Openapı belgesindeki her işlem, `operationId` değeri mevcut işlemin Azure Kaynak adı ile karşılaştırılarak mevcut işlemle eşleştirilir.

Bir eşleşme bulunursa, mevcut işlemin özellikleri "yerinde" güncelleştirilir.

Bir eşleşme bulunamazsa, yukarıdaki bölümde açıklanan kurallar kullanılarak yeni bir işlem oluşturulur. Her yeni işlem için, içeri aktarma işlemi aynı HTTP yöntemi ve yol şablonuyla mevcut bir işlemden ilke kopyalamaya çalışır.

Tüm mevcut eşleşmeyen işlemler silinecektir.

İçeri aktarmayı daha öngörülebilir hale getirmek için lütfen şu yönergeleri izleyin:

- Her işlem için `operationId` özelliğini belirttiğinizden emin olun.
- İlk içeri aktarma işleminden sonra `operationId` değiştirmaktan kaçının.
- `operationId` ve HTTP yöntemini veya yol şablonunu aynı anda değiştirmeyin.

### <a name="export-api-as-openapi"></a>API 'YI Openapı olarak dışarı aktar

Her işlem için, Azure Kaynak adı bir `operationId`olarak verilecek ve görünen ad bir `summary`olarak verilecek.
OperationId için normalleştirme kuralları

- Küçük harfe Dönüştür.
- Alfasayısal olmayan karakterlerin her dizisini tek bir çizgiyle değiştirin, örneğin `GET-/foo/{bar}?buzz={quix}` `get-foo-bar-buzz-quix-`dönüştürülür.
- Her iki tarafta da kesik çizgileri kırpın, örneğin `get-foo-bar-buzz-quix-` `get-foo-bar-buzz-quix` olur
- Bir kaynak adı için en fazla dört karakter olan 76 karakter uzunluğunda olacak şekilde kısaltın.
- Gerekirse, yinelenenleri kaldırma son eki için, `-1, -2, ..., -999`biçiminde kalan dört karakteri kullanın.


## <a name="wsdl"> </a>WSDL

WSDL dosyaları SOAP geçişli ve SOAP--REST API 'Leri oluşturmak için kullanılır.

-   **SOAP bağlamaları** -yalnızca "Document" ve "literal" kodlamasının SOAP bağlamaları desteklenir. "RPC" stili veya SOAP kodlaması desteği yoktur.
-   **Wsdl: Import** -bu öznitelik desteklenmiyor. Müşteriler içeri aktarmaları tek bir belgede birleştirmelidir.
-   **Birden çok parçaya sahip iletiler** -bu tür iletiler desteklenmez.
-   **WCF WSHttpBinding** -Windows Communication Foundation ile oluşturulan soap hizmetleri, BasicHttpBinding-WSHttpBinding kullanmalıdır.
-   **MTOM** -MTOM kullanan hizmetler işe <em>başlayabilir</em> . Resmi destek şu anda sunulmamaktadır.
-   Yinelemeli olarak tanımlanan **özyineleme** türleri (örneğin, kendi dizisine başvuru) APIM tarafından desteklenmez.
-   **Birden çok ad** alanı-bir şemada birden fazla ad alanı kullanılabilir, ancak ileti parçalarını tanımlamak için yalnızca hedef ad alanı kullanılabilir. Diğer giriş veya çıkış öğelerini tanımlamak için kullanılan hedeften farklı ad alanları korunmaz. Bu tür bir WSDL belgesi içeri aktarılabilse de, tüm ileti bölümlerinin dışarı aktarılması için WSDL 'nin hedef ad alanı olacaktır.
-   **Diziler** -SOAP-Rest dönüşümü, yalnızca aşağıdaki örnekte gösterilen sarmalanmış dizileri destekler:

```xml
    <complexType name="arrayTypeName">
        <sequence>
            <element name="arrayElementValue" type="arrayElementType" minOccurs="0" maxOccurs="unbounded"/>
        </sequence>
    </complexType>
    <complexType name="typeName">
        <sequence>
            <element name="element1" type="someTypeName" minOccurs="1" maxOccurs="1"/>
            <element name="element2" type="someOtherTypeName" minOccurs="0" maxOccurs="1" nillable="true"/>
            <element name="arrayElement" type="arrayTypeName" minOccurs="1" maxOccurs="1"/>
        </sequence>
    </complexType>
```

## <a name="wadl"> </a>Wadl

Şu anda bilinen bir WADL içeri aktarma sorunu yok.
