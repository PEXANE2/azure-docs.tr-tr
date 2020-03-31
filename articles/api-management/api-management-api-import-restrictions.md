---
title: API biçimlerinin kısıtlamaları ve ayrıntıları desteği
titleSuffix: Azure API Management
description: Azure API Yönetimi'nde Açık API, WSDL ve WADL biçimleriyle ilgili bilinen sorunlar ve kısıtlamaların ayrıntıları desteklenir.
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
ms.openlocfilehash: 61d43addfdf9008cb7aa8a073dcf3bb702cb55f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513380"
---
# <a name="api-import-restrictions-and-known-issues"></a>API içeri aktarma kısıtlamaları ve bilinen sorunlar

## <a name="about-this-list"></a>Bu liste hakkında

Bir API alırken, alma işlemi başarıyla gerçekleştirebilmeniz için bazı kısıtlamalarla karşılaşabilir veya düzeltilmesi gereken sorunları tanımlayabilirsiniz. Bu makalede, API'nin alma biçimine göre düzenlenen bu sınırlamalar belgelenmektedir. Ayrıca OpenAPI dışa aktarma nın nasıl çalıştığını da açıklar.

## <a name="openapiswagger-import-limitations"></a><a name="open-api"> </a>OpenAPI/Swagger alma sınırlamaları

OpenAPI belgenizi içe aktaran hatalar alıyorsanız, belgeyi önceden doğruladığınızdan emin olun. Bunu Azure portalındaki tasarımcıyı (Tasarım - Ön Uç - OpenAPI Belirtim Düzenleyicisi) veya <a href="https://editor.swagger.io">Swagger Editor</a>gibi bir üçüncü taraf aracıyla yapabilirsiniz.

### <a name="general"></a><a name="open-api-general"> </a>Genel

-   Hem yol hem de sorgu arasında gerekli parametrelerin benzersiz adları olmalıdır. (OpenAPI'de bir parametre adının yalnızca bir konum içinde benzersiz olması gerekir, örneğin yol, sorgu, üstbilgi. Ancak, API Yönetimi'nde operasyonların hem yol hem de sorgu parametreleri (OpenAPI'nin desteklemediği) tarafından ayrımcılığa uğramasına izin veririz. Bu nedenle, tüm URL şablonu içinde parametre adlarının benzersiz olmasını şart görüyoruz.)
-   `\$ref`işaretçiler dış dosyalara başvuru yapamaz.
-   `x-ms-paths`ve `x-servers` desteklenen tek uzantılarıvardır.
-   Özel uzantılar içe aktarıldır gözardı edilir ve dışa aktarılmak üzere kaydedilmez veya korunmaz.
-   `Recursion`- API Yönetimi özyinelemeli olarak tanımlanan tanımları desteklemez (örneğin, şemalar kendilerine atıfta bulunur).
-   Kaynak dosya URL'si (varsa) göreli sunucu URL'lerine uygulanır.
-   Güvenlik tanımları yoksayılır.
-   API işlemleri için satır satır şeması tanımları desteklenmez. Şema tanımları API kapsamında tanımlanır ve API işlemleri istek veya yanıt kapsamlarında başvurulabilir.
-   Tanımlı bir URL parametresi URL şablonunun bir parçası olmalıdır.
-   `Produces`API tarafından döndürülen MIME türlerini açıklayan anahtar sözcük desteklenmez. 

### <a name="openapi-version-2"></a><a name="open-api-v2"> </a>OpenAPI sürüm 2

-   Yalnızca JSON biçimi desteklenir.

### <a name="openapi-version-3"></a><a name="open-api-v3"> </a>OpenAPI sürüm 3

-   Çok `servers` sayıda belirtilmişse, API Yönetimi ilk HTTPs URL'sini seçmeye çalışır. Herhangi bir HTTPs URL'si yoksa - ilk HTTP URL. Herhangi bir HTTP URL'si yoksa , sunucu URL'si boş olacaktır.
-   `Examples`desteklenmez, ancak `example` desteklenir.

## <a name="openapi-import-update-and-export-mechanisms"></a>OpenAPI alma, güncelleme ve dışa aktarma mekanizmaları

### <a name="add-new-api-via-openapi-import"></a>OpenAPI alma yoluyla yeni API ekleme

OpenAPI belgesinde bulunan her işlem için, Azure kaynak adı ve görüntü `operationId` adı `summary` için ayarlanmış ve sırasıyla yeni bir işlem oluşturulur. `operationId`değeri aşağıda açıklanan kurallara göre normalleştirilmiştir. `summary`değeri olduğu gibi alınır ve uzunluğu 300 karakterle sınırlıdır.

`operationId` Belirtilmemişse (yani mevcut değil `null`veya boş), Azure kaynak adı değeri, örneğin HTTP yöntemi ve yol `get-foo`şablonu birleştirilerek oluşturulur.

`summary` Belirtilmemişse (yani, mevcut değil, `null`veya `display name` boş), değer `operationId`. `operationId` Belirtilmemişse, görüntüleme adı değeri HTTP yöntemi ve yol şablonu `Get - /foo`birleştirilerek oluşturulur, örneğin.

### <a name="update-an-existing-api-via-openapi-import"></a>OpenAPI alma yoluyla varolan bir API'yi güncelleştirme

Alma sırasında varolan API, OpenAPI belgesinde açıklanan API ile eşleşecek şekilde değiştirilir. OpenAPI belgesindeki her işlem, `operationId` değerini varolan işlemin Azure kaynak adı ile karşılaştırarak varolan işlemle eşleşir.

Bir eşleşme bulunursa, varolan işlemin özellikleri "yerinde" güncelleştirilir.

Bir eşleşme bulunmazsa, yukarıdaki bölümde açıklanan kurallar kullanılarak yeni bir işlem oluşturulur. Her yeni işlem için içe aktarma, varolan bir işlemdeki ilkeleri aynı HTTP yöntemi ve yol şablonuyla kopyalamaya çalışır.

Varolan tüm eşleşmez işlemler silinir.

Alma işleminin daha öngörülebilir hale getirmek için lütfen aşağıdaki yönergeleri izleyin:

- Her işlem `operationId` için özellik belirttiğinden emin olun.
- İlk alma `operationId` işleminden sonra değişmekten kaçının.
- Asla `operationId` ve HTTP yöntemi veya yol şablonu aynı anda değiştirin.

### <a name="export-api-as-openapi"></a>OpenAPI olarak API'yi dışa aktarma

Her işlem için Azure kaynak adı dışa `operationId`aktarılır ve görüntü adı `summary`.' olarak dışa aktarılır.
OperationId için normalleştirme kuralları

- Küçük harfe dönüştürün.
- Alfasayısal olmayan karakterlerin her dizisini tek bir çizgiyle değiştirin, örneğin, `GET-/foo/{bar}?buzz={quix}` . `get-foo-bar-buzz-quix-`
- Örneğin, `get-foo-bar-buzz-quix-` her iki taraftaki tireler`get-foo-bar-buzz-quix`
- Bir kaynak adı için maksimum sınırdan dört karakter daha az, 76 karakter sığdırmak için truncate.
- Bir deduplication soneki için kalan dört karakter kullanın, `-1, -2, ..., -999`gerekirse, şeklinde .


## <a name="wsdl"></a><a name="wsdl"> </a>WSDL

WSDL dosyaları SOAP pass-through ve SOAP-to-REST API'leri oluşturmak için kullanılır.

-   **SOAP ciltleri** -Sadece stil "belge" ve "literal" kodlama SOAP ciltler desteklenir. "rpc" stili veya SOAP-Kodlama için destek yoktur.
-   **WSDL:İçe Aktarma** - Bu öznitelik desteklenmez. Müşteriler içeri aktarımları tek bir belgede birleştirmelidir.
-   **Birden çok parçalı iletiler** - Bu tür iletiler desteklenmez.
-   **WCF wsHttpBinding** - Windows Communication Foundation ile oluşturulan SOAP hizmetleri temel HttpBinding kullanmalıdır - wsHttpBinding desteklenmez.
-   **MTOM** - MTOM kullanan hizmetler <em>çalışabilir.</em> Şu anda resmi destek sunulmadı.
-   **Özyineleme** - Özyinelemeli olarak tanımlanan türler (örneğin, kendilerine bir dizi bakın) APIM tarafından desteklenmez.
-   **Birden Çok Ad Alanı** - Şema'da birden çok ad alanı kullanılabilir, ancak ileti bölümlerini tanımlamak için yalnızca hedef ad alanı kullanılabilir. Diğer giriş veya çıktı öğelerini tanımlamak için kullanılan hedef dışındaki ad alanları korunmaz. Böyle bir WSDL belgesi içe aktarılabilse de, dışa aktarmada tüm ileti parçaları WSDL'nin hedef ad alanına sahip olacaktır.
-   **Diziler** - SOAP-to-REST dönüşümü yalnızca aşağıdaki örnekte gösterilen sarılmış dizileri destekler:

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

## <a name="wadl"></a><a name="wadl"> </a>WADL

Şu anda bilinen WADL alma sorunları yok.
