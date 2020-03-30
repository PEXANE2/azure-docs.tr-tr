---
title: Azure API Yönetimi gelişmiş ilkeleri | Microsoft Dokümanlar
description: Azure API Yönetimi'nde kullanılmak üzere kullanılabilen gelişmiş ilkeler hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/10/2020
ms.author: apimpm
ms.openlocfilehash: c8ef481fe277d6451923da828f0e7473354c24cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266175"
---
# <a name="api-management-advanced-policies"></a>API Management gelişmiş ilkeleri

Bu konu, aşağıdaki API Yönetimi ilkeleri için bir başvuru sağlar. İlkeler ekleme ve yapılandırma hakkında bilgi için [API Yönetimi'ndeki İlkeler'e](https://go.microsoft.com/fwlink/?LinkID=398186)bakın.

## <a name="advanced-policies"></a><a name="AdvancedPolicies"></a>Gelişmiş ilkeler

-   [Kontrol akışı](api-management-advanced-policies.md#choose) - Koşullu Boolean ifadelerinin değerlendirilmesi sonuçlarına dayalı ilke [deyimleri](api-management-policy-expressions.md)uygular.
-   [İleri isteği](#ForwardRequest) - İsteği arka uç hizmetine iletin.
-   [Eşzamanlılığı sınırlayın](#LimitConcurrency) - Kapalı ilkelerin bir seferde belirtilen sayıdan daha fazla istekle yürütülmesini engeller.
-   [Olay Hub'ına Giriş](#log-to-eventhub) - Logger varlığı tarafından tanımlanan bir Olay Hub'ına belirtilen biçimde ileti gönderir.
-   [Mock yanıt](#mock-response) - Ardışık hatlar yürütmea aborts ve doğrudan arayan bir mocked yanıt döndürür.
-   [Yeniden Deneme](#Retry) - Koşul yerine getirile kadar ekteki ilke deyimlerinin yürütülmesini yeniden dener. Yürütme, belirtilen zaman aralıklarında ve belirtilen yeniden deneme sayısına kadar yinelenecektir.
-   [İade yanıtı](#ReturnResponse) - Ardışık hatlar yürütmesini iptal eder ve belirtilen yanıtı doğrudan arayana döndürür.
-   [Tek yönlü istek gönder](#SendOneWayRequest) - Yanıt beklemeden belirtilen URL'ye istek gönderir.
-   [İstek gönder](#SendRequest) - Belirtilen URL'ye istek gönderir.
-   [HTTP proxy'yi ayarlayın](#SetHttpProxy) - İlegönderilen istekleri http proxy üzerinden yönlendirmenizi sağlar.
-   [İstek yöntemini ayarla](#SetRequestMethod) - İstek için HTTP yöntemini değiştirmenizi sağlar.
-   [Durum kodunu ayarla](#SetStatus) - HTTP durum kodunu belirtilen değerle değiştirir.
-   [Değişkeni Ayarla](api-management-advanced-policies.md#set-variable) - Daha sonraki erişim için [adlandırılmış](api-management-policy-expressions.md#ContextVariables) bağlam değişkeninde bir değer kalır.
-   [İzleme](#Trace) - [API Denetçisi](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) çıktısına, Uygulama Öngörüleri telemelerine ve Tanıgünlüklerine özel izlemeler ekler.
-   [Bekle](#Wait) - Kapalı [Gönder isteği](api-management-advanced-policies.md#SendRequest)için bekler, [önbellekten değer alın](api-management-caching-policies.md#GetFromCacheByKey)veya devam etmeden önce tamamlanması gereken akış ilkelerini [denetle.](api-management-advanced-policies.md#choose)

## <a name="control-flow"></a><a name="choose"></a>Kontrol akışı

İlke, `choose` boolean ifadelerinin değerlendirilmesinin sonucuna dayalı olarak, programlama dilinde if-then veya switch yapısına benzer kapalı ilke deyimleri uygular.

### <a name="policy-statement"></a><a name="ChoosePolicyStatement"></a>İlke bildirimi

```xml
<choose>
    <when condition="Boolean expression | Boolean constant">
        <!— one or more policy statements to be applied if the above condition is true  -->
    </when>
    <when condition="Boolean expression | Boolean constant">
        <!— one or more policy statements to be applied if the above condition is true  -->
    </when>
    <otherwise>
        <!— one or more policy statements to be applied if none of the above conditions are true  -->
</otherwise>
</choose>
```

Denetim akışı ilkesi en az `<when/>` bir öğe içermelidir. Öğe `<otherwise/>` isteğe bağlıdır. Elemanlardaki `<when/>` koşullar, ilke içinde göründükleri sırayla değerlendirilir. İlk `<when/>` öğe nin içinde koşul özniteliği eşit olan `true` ilk öğe(ler) uygulanacaktır. Öğenin içinde kapalı ilkeler, varsa, tüm öğe koşul öznitelikleri `false`ise uygulanacaktır. `<otherwise/>` `<when/>`

### <a name="examples"></a>Örnekler

#### <a name="example"></a><a name="ChooseExample"></a>Örnek

Aşağıdaki örnek, bir [set-değişken](api-management-advanced-policies.md#set-variable) ilkesi ve iki denetim akışı ilkesini gösterir.

Ayarlanan değişken ilkesi gelen bölümdedir `isMobile` ve [context](api-management-policy-expressions.md#ContextVariables) `User-Agent` istek üstbilgisi metni `iPad` içeriyorsa veya . `iPhone`

İlk denetim akışı ilkesi de gelen bölümdedir ve `isMobile` bağlam değişkeninin değerine bağlı olarak iki Set sorgusu [dize parametre](api-management-transformation-policies.md#SetQueryStringParameter) ilkesinden birini koşullu olarak uygular.

İkinci kontrol akışı ilkesi giden bölümdedir ve ''' olarak ayarlandığında `isMobile` [XML'i JSON ilkesine dönüştürme](api-management-transformation-policies.md#ConvertXMLtoJSON) yi koşullu olarak `true`uygular.

```xml
<policies>
    <inbound>
        <set-variable name="isMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />
        <base />
        <choose>
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">
                <set-query-parameter name="mobile" exists-action="override">
                    <value>true</value>
                </set-query-parameter>
            </when>
            <otherwise>
                <set-query-parameter name="mobile" exists-action="override">
                    <value>false</value>
                </set-query-parameter>
            </otherwise>
        </choose>
    </inbound>
    <outbound>
        <base />
        <choose>
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>
            </when>
        </choose>
    </outbound>
</policies>
```

#### <a name="example"></a>Örnek

Bu örnek, ürünü kullanırken arka uç hizmetinden alınan yanıttan veri öğelerini `Starter` kaldırarak içerik filtrelemenin nasıl gerçekleştirildiğini gösterir. Bu politikayı yapılandırma ve kullanma nın bir gösterimi [için, Vlad Vinogradsky ile Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 34:30'a kadar ileri sarma bölümüne bakın. Bu demo için kullanılan Karanlık Gökyüzü [Tahmini API'sinin](https://developer.forecast.io/) genel görünümünü görmek için 31:50'den başlayın.

```xml
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->
<choose>
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">
    <set-body>@{
        var response = context.Response.Body.As<JObject>();
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {
          response.Property (key).Remove ();
        }
        return response.ToString();
      }
    </set-body>
  </when>
</choose>
```

### <a name="elements"></a>Öğeler

| Öğe   | Açıklama                                                                                                                                                                                                                                                               | Gerekli |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| Seçin    | Kök öğesi.                                                                                                                                                                                                                                                             | Evet      |
| Tesis      | İlkenin `if` veya `ifelse` bazı kısımlarıiçin kullanılacak koşul. `choose` İlkenin `choose` birden `when` çok bölümü varsa, bunlar sırayla değerlendirilir. `condition` Bir kez bir öğe nin `true`değerlendirilmesi, başka `when` koşullar değerlendirilir. | Evet      |
| Aksi takdir -de | `when` Hiçbir koşul da değerlendirilmezse kullanılacak ilke snippet'ini `true`içerir.                                                                                                                                                                               | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Öznitelik                                              | Açıklama                                                                                               | Gerekli |
| ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- | -------- |
| koşul="Boolean ekspresyonu &#124; Boolean sabiti" | İçeren `when` ilke deyimi değerlendirildiğinde Boolean ifadesi veya sabiti değerlendirilir. | Evet      |

### <a name="usage"></a><a name="ChooseUsage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="forward-request"></a><a name="ForwardRequest"></a>İleri isteği

İlke, `forward-request` gelen isteği istek [bağlamında](api-management-policy-expressions.md#ContextVariables)belirtilen arka uç hizmetine ileter. Arka uç hizmet URL'si API [ayarlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) belirtilir ve [set backend servis](api-management-transformation-policies.md) ilkesi kullanılarak değiştirilebilir.

> [!NOTE]
> Bu ilkenin kaldırılması, isteğin arka uç hizmetine iletilmesine neden olur ve giden bölümdeki ilkeler, gelen bölümdeki ilkelerin başarıyla tamamlanmasından hemen sonra değerlendirilir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<forward-request timeout="time in seconds" follow-redirects="false | true" buffer-request-body="false | true" fail-on-error-status-code="false | true"/>
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

Aşağıdaki API düzeyi ilkesi, tüm API isteklerini 60 saniyelik bir zaman aralığıyla arka uç hizmetine ileter.

```xml
<!-- api level -->
<policies>
    <inbound>
        <base/>
    </inbound>
    <backend>
        <forward-request timeout="60"/>
    </backend>
    <outbound>
        <base/>
    </outbound>
</policies>

```

#### <a name="example"></a>Örnek

Bu işlem düzeyi `base` ilkesi, üst uç ilkesini üst API düzey kapsamından devralmak için öğeyi kullanır.

```xml
<!-- operation level -->
<policies>
    <inbound>
        <base/>
    </inbound>
    <backend>
        <base/>
    </backend>
    <outbound>
        <base/>
    </outbound>
</policies>

```

#### <a name="example"></a>Örnek

Bu işlem düzeyi ilkesi, tüm istekleri 120'lik bir zaman dilimiyle açıkça arka uç hizmetine ileter ve üst API düzeyi arka uç ilkesini devralmaz. Arka uç hizmeti 400 ile 599 arasında bir hata durum koduyla yanıt verirse, [hata üzerine](api-management-error-handling-policies.md) bölümü tetiklenir.

```xml
<!-- operation level -->
<policies>
    <inbound>
        <base/>
    </inbound>
    <backend>
        <forward-request timeout="120" fail-on-error-status-code="true" />
        <!-- effective policy. note the absence of <base/> -->
    </backend>
    <outbound>
        <base/>
    </outbound>
</policies>

```

#### <a name="example"></a>Örnek

Bu işlem düzeyi ilkesi istekleri arka uç hizmetine iletmez.

```xml
<!-- operation level -->
<policies>
    <inbound>
        <base/>
    </inbound>
    <backend>
        <!-- no forwarding to backend -->
    </backend>
    <outbound>
        <base/>
    </outbound>
</policies>

```

### <a name="elements"></a>Öğeler

| Öğe         | Açıklama   | Gerekli |
| --------------- | ------------- | -------- |
| ileri istek | Kök öğesi. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik                                     | Açıklama                                                                                                                                                                                                                                                                                                    | Gerekli | Varsayılan |
| --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| zaman aşım="sonda"                             | Bir zaman sonu hatası yükseltilmeden önce HTTP yanıt üstbilgilerinin arka uç hizmeti tarafından döndürülmesini beklemek için saniye cinsinden süre. Minimum değer 0 saniyedir. 240 saniyeden büyük değerler, temel ağ altyapısı bu saatten sonra boşta kalan bağlantıları bırakabileceğinden onurlandırılamayabilir. | Hayır       | None    |
| follow-yönlendirme="false &#124; true"          | Arka uç hizmetinden yönlendirmelerin ağ geçidi tarafından izlenip izlenmediğini veya arayanın döndürülüp döndürülmeyeceğini belirtir.                                                                                                                                                                                                    | Hayır       | yanlış   |
| tampon-istek-gövde="yanlış &#124; doğru"       | "True" isteğine ayarlandığında arabelleğe alınıp [yeniden denemede](api-management-advanced-policies.md#Retry)yeniden kullanılacaktır.                                                                                                                                                                                               | Hayır       | yanlış   |
| fail-on-error-status-code="false &#124; true" | 400 ile 599 arasında değişen yanıt kodları için hata [üzerine](api-management-error-handling-policies.md) bölüm gerçek olarak ayarlandığında.                                                                                                                                                                      | Hayır       | yanlış   |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** arka uç
-   **İlke kapsamları:** tüm kapsamlar

## <a name="limit-concurrency"></a><a name="LimitConcurrency"></a>Eşzamanlılığı sınırlandırın

İlke, `limit-concurrency` kapalı ilkelerin herhangi bir zamanda belirtilen sayıdan daha fazla istekle yürütülmesini engeller. Bu sayıyı aştıktan sonra, 429 Çok Fazla İstek durum koduyla yeni istekler hemen başarısız olur.

### <a name="policy-statement"></a><a name="LimitConcurrencyStatement"></a>İlke bildirimi

```xml
<limit-concurrency key="expression" max-count="number">
        <!— nested policy statements -->
</limit-concurrency>
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

Aşağıdaki örnek, bağlam değişkeninin değerini temel alan bir arka uça iletilen istek sayısını nasıl sınırlandırılabildiğini gösterir.

```xml
<policies>
  <inbound>…</inbound>
  <backend>
    <limit-concurrency key="@((string)context.Variables["connectionId"])" max-count="3">
      <forward-request timeout="120"/>
    <limit-concurrency/>
  </backend>
  <outbound>…</outbound>
</policies>
```

### <a name="elements"></a>Öğeler

| Öğe           | Açıklama   | Gerekli |
| ----------------- | ------------- | -------- |
| limit-eşzamanlılık | Kök öğesi. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik | Açıklama                                                                                        | Gerekli | Varsayılan |
| --------- | -------------------------------------------------------------------------------------------------- | -------- | ------- |
| anahtar       | Bir ip. İfade ye izin verildi. Eşzamanlılık kapsamını belirtir. Birden çok ilke tarafından paylaşılabilir. | Evet      | Yok     |
| maksimum sayım | Bir tamsayı. İlkeyi girmesine izin verilen en fazla istek sayısını belirtir.           | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="log-to-event-hub"></a><a name="log-to-eventhub"></a>Etkinlik Hub'ına Giriş Yap

İlke, `log-to-eventhub` logger varlığı tarafından tanımlanan bir Olay Hub'ına belirtilen biçimde iletigönderir. Adından da anlaşılacağı gibi, ilke, çevrimiçi veya çevrimdışı çözümlemesi için seçili istek veya yanıt bağlamı bilgilerini kaydetmek için kullanılır.

> [!NOTE]
> Bir etkinlik hub'ı yapılandırma ve olayları günlüğe kaydetme yle ilgili adım adım kılavuz [için, API Yönetimi etkinliklerini Azure Etkinlik Hub'larıyla nasıl günlüğe kaydedin.](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/)

### <a name="policy-statement"></a>İlke bildirimi

```xml
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">
  Expression returning a string to be logged
</log-to-eventhub>

```

### <a name="example"></a>Örnek

Herhangi bir dize Olay Hub'larında günlüğe kaydedilecek değer olarak kullanılabilir. Bu örnekte, gelen tüm aramalar için tarih ve saat, dağıtım hizmet adı, istek kimliği, IP adresi ve `contoso-logger` işlem adı, kimlikle kayıtlı olay merkezi Logger'a günlüğe kaydedilir

```xml
<policies>
  <inbound>
    <log-to-eventhub logger-id ='contoso-logger'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name) )
    </log-to-eventhub>
  </inbound>
  <outbound>
  </outbound>
</policies>
```

### <a name="elements"></a>Öğeler

| Öğe         | Açıklama                                                                     | Gerekli |
| --------------- | ------------------------------------------------------------------------------- | -------- |
| log-to-eventhub | Kök öğesi. Bu öğenin değeri, olay hub'ınıza günlüğe kaydolacak dizedir. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik     | Açıklama                                                               | Gerekli                                                             |
| ------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| logger-id     | API Yönetimi hizmetinize kayıtlı Logger kimliği.         | Evet                                                                  |
| bölüm-id  | İletilerin gönderildiği bölümün dizinini belirtir.             | İsteğe bağlı. Bu öznitelik kullanılırsa `partition-key` kullanılmayabilir. |
| bölüm anahtarı | İletiler gönderildiğinde bölüm ataması için kullanılan değeri belirtir. | İsteğe bağlı. Bu öznitelik kullanılırsa `partition-id` kullanılmayabilir.  |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="mock-response"></a><a name="mock-response"></a>Sahte yanıt

Adından `mock-response`da anlaşılacağı gibi, API'ler ve işlemler alay etmek için kullanılır. Normal ardışık hat lar yürütmesini iptal eder ve arayana alaycı bir yanıt verir. İlke her zaman en yüksek sadakat yanıtlarını döndürmeye çalışır. Kullanılabilir olduğunda yanıt içeriği örneklerini tercih eder. Şemalar sağlandığında ve örnekler verilmediğinde şemalardan örnek yanıtlar üretir. Örnekler veya şemalar yoksa, içeriği olmayan yanıtlar döndürülür.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<mock-response status-code="code" content-type="media type"/>

```

### <a name="examples"></a>Örnekler

```xml
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this
status code. First found content type is used. If no example or schema is found, the content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this
status code and media type. If no example or schema found, the content is empty. -->
<mock-response status-code='200' content-type='application/json'/>
```

### <a name="elements"></a>Öğeler

| Öğe       | Açıklama   | Gerekli |
| ------------- | ------------- | -------- |
| mock-yanıt | Kök öğesi. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik    | Açıklama                                                                                           | Gerekli | Varsayılan |
| ------------ | ----------------------------------------------------------------------------------------------------- | -------- | ------- |
| durum kodu  | Yanıt durum kodunu belirtir ve ilgili örneği veya şemayı seçmek için kullanılır.                 | Hayır       | 200     |
| içerik türü | Yanıt üstbilgi değerini belirtir `Content-Type` ve ilgili örneği veya şemayı seçmek için kullanılır. | Hayır       | None    |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="retry"></a><a name="Retry"></a>Yeni -den deneme

`retry` İlke, alt ilkelerini bir kez yürütür ve yeniden `condition` `false` deneme bitene veya yeniden deneme `count` bitene kadar yürütmelerini yeniden dener.

### <a name="policy-statement"></a>İlke bildirimi

```xml

<retry
    condition="boolean expression or literal"
    count="number of retry attempts"
    interval="retry interval in seconds"
    max-interval="maximum retry interval in seconds"
    delta="retry interval delta in seconds"
    first-fast-retry="boolean expression or literal">
        <!-- One or more child policies. No restrictions -->
</retry>

```

### <a name="example"></a>Örnek

Aşağıdaki örnekte, istek iletme üstel yeniden deneme algoritması kullanılarak on kata kadar yeniden denenmiş olur. Yanlış `first-fast-retry` olarak ayarlıolduğundan, tüm yeniden deneme girişimleri üstel yeniden deneme algoritmasına tabidir.

```xml

<retry
    condition="@(context.Response.StatusCode == 500)"
    count="10"
    interval="10"
    max-interval="100"
    delta="10"
    first-fast-retry="false">
        <forward-request buffer-request-body="true" />
</retry>

```

### <a name="elements"></a>Öğeler

| Öğe | Açıklama                                                         | Gerekli |
| ------- | ------------------------------------------------------------------- | -------- |
| retry   | Kök öğesi. Alt öğeleri olarak başka ilkeler içerebilir. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik        | Açıklama                                                                                                                                           | Gerekli | Varsayılan |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| Durum        | Yeniden denemelerin durdurulup durdurulmaması gerektiğini belirten bir`false`boolean`true`literal veya [ifade](api-management-policy-expressions.md) ( ) veya devam ( ).      | Evet      | Yok     |
| count            | Denemek için en fazla yeniden deneme sayısını belirten pozitif bir sayı.                                                                                | Evet      | Yok     |
| interval         | Yeniden deneme denemeleri arasındaki bekleme aralığını belirten saniye cinsinden pozitif bir sayı.                                                                 | Evet      | Yok     |
| maksimum aralık     | Yeniden deneme denemeleri arasındaki maksimum bekleme aralığını belirten saniye cinsinden pozitif bir sayı. Üstel yeniden deneme algoritması uygulamak için kullanılır. | Hayır       | Yok     |
| Delta            | Bekleme aralığı artışını belirten saniye cinsinden pozitif bir sayı. Doğrusal ve üstel yeniden deneme algoritmalarını uygulamak için kullanılır.             | Hayır       | Yok     |
| ilk hızlı-yeniden deneme | `true` Ayarlanırsa, ilk yeniden deneme girişimi hemen gerçekleştirilir.                                                                                  | Hayır       | `false` |

> [!NOTE]
> Yalnızca belirtildiğinde, `interval` **sabit** aralık lı yeniden denemeler gerçekleştirilir.
> Yalnızca `interval` ve `delta` belirtildiğinde, **doğrusal** aralık lı yeniden deneme algoritması kullanılır ve burada yeniden `interval + (count - 1)*delta`denemeler arasındaki bekleme süresi aşağıdaki formüle göre hesaplanır .
> `interval` `max-interval` , ve `delta` belirtildiğinde, **üstel** aralık yeniden deneme algoritması uygulandığında, yeniden denemeler arasındaki bekleme süresi aşağıdaki `interval` formüle göre `max-interval` değerdeğerinden katlanarak `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`büyüyor - .

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) kullanılabilir. Alt ilke kullanım kısıtlamalarının bu ilke tarafından devralınacağını unutmayın.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="return-response"></a><a name="ReturnResponse"></a>İade yanıtı

İlke, `return-response` ardışık hatlar yürütmesini iptal eder ve arayana varsayılan veya özel bir yanıt verir. Varsayılan yanıt `200 OK` gövdesizdir. Özel yanıt bir bağlam değişkeni veya ilke deyimleri ile belirtilebilir. Her ikisi de sağlandığında, bağlam değişkeninde yer alan yanıt, arayana iade edilmeden önce ilke deyimleri tarafından değiştirilir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<return-response response-variable-name="existing context variable">
  <set-header/>
  <set-body/>
  <set-status/>
</return-response>

```

### <a name="example"></a>Örnek

```xml
<return-response>
   <set-status code="401" reason="Unauthorized"/>
   <set-header name="WWW-Authenticate" exists-action="override">
      <value>Bearer error="invalid_token"</value>
   </set-header>
</return-response>

```

### <a name="elements"></a>Öğeler

| Öğe         | Açıklama                                                                               | Gerekli |
| --------------- | ----------------------------------------------------------------------------------------- | -------- |
| return-response | Kök öğesi.                                                                             | Evet      |
| set-üstbilgi      | [Bir ayar üstbilgi](api-management-transformation-policies.md#SetHTTPheader) ilkesi deyimi. | Hayır       |
| set gövdesi        | Belirli bir [ayar](api-management-transformation-policies.md#SetBody) lı politika bildirimi.         | Hayır       |
| set durumu      | [Bir ayar durumu](api-management-advanced-policies.md#SetStatus) ilkesi deyimi.           | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Öznitelik              | Açıklama                                                                                                                                                                          | Gerekli  |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- |
| yanıt-değişken-isim | Örneğin, bir yukarı [gönderme isteği](api-management-advanced-policies.md#SendRequest) ilkesinden başvurulan bağlam değişkeninin `Response` adı ve bir nesne içeren | İsteğe bağlı. |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="send-one-way-request"></a><a name="SendOneWayRequest"></a>Tek yönlü istek gönder

İlke, `send-one-way-request` sağlanan isteği yanıt beklemeden belirtilen URL'ye gönderir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<send-one-way-request mode="new | copy">
  <url>...</url>
  <method>...</method>
  <header name="" exists-action="override | skip | append | delete">...</header>
  <body>...</body>
  <authentication-certificate thumbprint="thumbprint" />
</send-one-way-request>

```

### <a name="example"></a>Örnek

Bu örnek ilke, HTTP `send-one-way-request` yanıt kodu 500'den büyük veya eşitse, Slack sohbet odasına ileti göndermek için ilkeyi kullanma nın bir örneğini gösterir. Bu örnek hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)

```xml
<choose>
    <when condition="@(context.Response.StatusCode >= 500)">
      <send-one-way-request mode="new">
        <set-url>https://hooks.slack.com/services/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</set-url>
        <set-method>POST</set-method>
        <set-body>@{
                return new JObject(
                        new JProperty("username","APIM Alert"),
                        new JProperty("icon_emoji", ":ghost:"),
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                context.Request.Method,
                                                context.Request.Url.Path + context.Request.Url.QueryString,
                                                context.Request.Url.Host,
                                                context.Response.StatusCode,
                                                context.Response.StatusReason,
                                                context.User.Email
                                                ))
                        ).ToString();
            }</set-body>
      </send-one-way-request>
    </when>
</choose>

```

### <a name="elements"></a>Öğeler

| Öğe                    | Açıklama                                                                                                 | Gerekli                        |
| -------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------- |
| gönderme-tek yönlü-istek       | Kök öğesi.                                                                                               | Evet                             |
| url                        | İsteğin URL'si.                                                                                     | Hayır if mode=kopya; aksi takdirde evet. |
| method                     | İstek için HTTP yöntemi.                                                                            | Hayır if mode=kopya; aksi takdirde evet. |
| üst bilgi                     | Üstbilgi isteyin. Birden çok istek üstbilgi için birden çok üstbilgi öğesi kullanın.                                  | Hayır                              |
| body                       | İstek organı.                                                                                           | Hayır                              |
| kimlik doğrulama sertifikası | [İstemci kimlik doğrulaması için kullanılacak sertifika](api-management-authentication-policies.md#ClientCertificate) | Hayır                              |

### <a name="attributes"></a>Öznitelikler

| Öznitelik     | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan  |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------- |
| mode="string" | Bunun yeni bir istek mi yoksa geçerli isteğin bir kopyası mı olduğunu belirler. Giden modda, mode=copy istek gövdesini başlatmaz.                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Hayır       | Yeni      |
| ad          | Ayarlanacak üst bilginin adını belirtir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Evet      | Yok      |
| var-eylem | Üstbilgi zaten belirtildiğinde hangi eylemin ne zaman gerçekleşip gerçekleşsin diye belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> - geçersiz kılma - varolan üstbilginin değerini değiştirir.<br />- atla - varolan üstbilgi değerinin yerini almaz.<br />- ek - varolan üstbilgi değerine değer ekler.<br />- sil - üstbilgi isteği kaldırır.<br /><br /> Aynı ada sahip birden çok giriş kaydedilecek `override` şekilde ayarlandığında, üstbilginin tüm girişlere göre ayarlandığı (birden çok kez listelenecek); yalnızca listelenen değerler sonuç olarak ayarlanır. | Hayır       | override |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="send-request"></a><a name="SendRequest"></a>İstek gönder

İlke, `send-request` sağlanan isteği belirtilen URL'ye, belirlenen zaman aşımı değerinden daha uzun beklemeden gönderir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error
="false|true">
  <set-url>...</set-url>
  <set-method>...</set-method>
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>
  <set-body>...</set-body>
  <authentication-certificate thumbprint="thumbprint" />
</send-request>

```

### <a name="example"></a>Örnek

Bu örnek, bir yetkilendirme sunucusuyla başvuru belirteci doğrulamak için bir yol gösterir. Bu örnek hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)

```xml
<inbound>
  <!-- Extract Token from Authorization header parameter -->
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

  <!-- Send request to Token Server to validate token (see RFC 7662) -->
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
    <set-method>POST</set-method>
    <set-header name="Authorization" exists-action="override">
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
    </set-header>
    <set-header name="Content-Type" exists-action="override">
      <value>application/x-www-form-urlencoded</value>
    </set-header>
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
  </send-request>

  <choose>
        <!-- Check active property in response -->
        <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
            <!-- Return 401 Unauthorized with http-problem payload -->
            <return-response>
                <set-status code="401" reason="Unauthorized" />
                <set-header name="WWW-Authenticate" exists-action="override">
                    <value>Bearer error="invalid_token"</value>
                </set-header>
            </return-response>
        </when>
    </choose>
  <base />
</inbound>

```

### <a name="elements"></a>Öğeler

| Öğe                    | Açıklama                                                                                                 | Gerekli                        |
| -------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------- |
| gönderme-istek               | Kök öğesi.                                                                                               | Evet                             |
| url                        | İsteğin URL'si.                                                                                     | Hayır if mode=kopya; aksi takdirde evet. |
| method                     | İstek için HTTP yöntemi.                                                                            | Hayır if mode=kopya; aksi takdirde evet. |
| üst bilgi                     | Üstbilgi isteyin. Birden çok istek üstbilgi için birden çok üstbilgi öğesi kullanın.                                  | Hayır                              |
| body                       | İstek organı.                                                                                           | Hayır                              |
| kimlik doğrulama sertifikası | [İstemci kimlik doğrulaması için kullanılacak sertifika](api-management-authentication-policies.md#ClientCertificate) | Hayır                              |

### <a name="attributes"></a>Öznitelikler

| Öznitelik                       | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan  |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------- |
| mode="string"                   | Bunun yeni bir istek mi yoksa geçerli isteğin bir kopyası mı olduğunu belirler. Giden modda, mode=copy istek gövdesini başlatmaz.                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Hayır       | Yeni      |
| yanıt-değişken-adı="string" | Yanıt nesnesi alacak bağlam değişkeninin adı. Değişken yoksa, ilke başarılı bir şekilde yürütülmesi üzerine oluşturulur ve toplama [`context.Variable`](api-management-policy-expressions.md#ContextVariables) yoluyla erişilebilir hale gelir.                                                                                                                                                                                                                                                                                                                          | Evet      | Yok      |
| zaman aşım="sonda"               | URL'ye çağrı başarısız olmadan saniyeler içinde zaman aralığı.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Hayır       | 60       |
| yoksay-hata                    | Doğruysa ve istek bir hataya neden oldu:<br /><br /> - Yanıt-değişken adı belirtilmişse, null bir değer içerir.<br />- Yanıt-değişken adı belirtilmediyse, bağlam. İstek güncelleştirilmeyecektir.                                                                                                                                                                                                                                                                                                                                                                                   | Hayır       | yanlış    |
| ad                            | Ayarlanacak üst bilginin adını belirtir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Evet      | Yok      |
| var-eylem                   | Üstbilgi zaten belirtildiğinde hangi eylemin ne zaman gerçekleşip gerçekleşsin diye belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> - geçersiz kılma - varolan üstbilginin değerini değiştirir.<br />- atla - varolan üstbilgi değerinin yerini almaz.<br />- ek - varolan üstbilgi değerine değer ekler.<br />- sil - üstbilgi isteği kaldırır.<br /><br /> Aynı ada sahip birden çok giriş kaydedilecek `override` şekilde ayarlandığında, üstbilginin tüm girişlere göre ayarlandığı (birden çok kez listelenecek); yalnızca listelenen değerler sonuç olarak ayarlanır. | Hayır       | override |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="set-http-proxy"></a><a name="SetHttpProxy"></a>HTTP proxy'yi ayarla

İlke, `proxy` http proxy ile gönderilen istekleri geri uçlara yönlendirmenize olanak tanır. Ağ geçidi ve proxy arasında yalnızca HTTP (HTTPS değil) desteklenir. Yalnızca Temel ve NTLM kimlik doğrulaması.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<proxy url="http://hostname-or-ip:port" username="username" password="password" />

```

### <a name="example"></a>Örnek

İlke belgesinde hassas bilgilerin depolanmasını önlemek için [özelliklerin](api-management-howto-properties.md) kullanıcı adı ve parola değerleri olarak kullanılmasına dikkat edin.

```xml
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />

```

### <a name="elements"></a>Öğeler

| Öğe | Açıklama  | Gerekli |
| ------- | ------------ | -------- |
| proxy   | Kök öğesi | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik         | Açıklama                                            | Gerekli | Varsayılan |
| ----------------- | ------------------------------------------------------ | -------- | ------- |
| url="string"      | Proxy URL şeklinde http://host:port.             | Evet      | Yok     |
| kullanıcı adı="string" | Proxy ile kimlik doğrulama için kullanılacak kullanıcı adı. | Hayır       | Yok     |
| password="string" | Proxy ile kimlik doğrulama için kullanılacak parola. | Hayır       | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen

-   **İlke kapsamları:** tüm kapsamlar

## <a name="set-request-method"></a><a name="SetRequestMethod"></a>İstek yöntemini ayarlama

İlke, `set-method` bir istek için HTTP istek yöntemini değiştirmenize olanak tanır.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-method>METHOD</set-method>

```

### <a name="example"></a>Örnek

İlkeyi `set-method` kullanan bu örnek ilke, HTTP yanıt kodu 500'den büyük veya eşitse, Slack sohbet odasına ileti gönderme örneğini gösterir. Bu örnek hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)

```xml
<choose>
    <when condition="@(context.Response.StatusCode >= 500)">
      <send-one-way-request mode="new">
        <set-url>https://hooks.slack.com/services/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</set-url>
        <set-method>POST</set-method>
        <set-body>@{
                return new JObject(
                        new JProperty("username","APIM Alert"),
                        new JProperty("icon_emoji", ":ghost:"),
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                context.Request.Method,
                                                context.Request.Url.Path + context.Request.Url.QueryString,
                                                context.Request.Url.Host,
                                                context.Response.StatusCode,
                                                context.Response.StatusReason,
                                                context.User.Email
                                                ))
                        ).ToString();
            }</set-body>
      </send-one-way-request>
    </when>
</choose>

```

### <a name="elements"></a>Öğeler

| Öğe    | Açıklama                                                       | Gerekli |
| ---------- | ----------------------------------------------------------------- | -------- |
| set yöntemi | Kök öğesi. Öğenin değeri HTTP yöntemini belirtir. | Evet      |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="set-status-code"></a><a name="SetStatus"></a>Durum kodunu ayarlama

İlke, `set-status` HTTP durum kodunu belirtilen değere ayarlar.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-status code="" reason=""/>

```

### <a name="example"></a>Örnek

Bu örnek, yetkilendirme belirteci geçersizse 401 yanıtının nasıl döndürülecek olduğunu gösterir. Daha fazla bilgi için bkz: [Azure API Yönetimi hizmetinden harici hizmetleri kullanma](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)

```xml
<choose>
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
    <return-response response-variable-name="existing response variable">
      <set-status code="401" reason="Unauthorized" />
      <set-header name="WWW-Authenticate" exists-action="override">
        <value>Bearer error="invalid_token"</value>
      </set-header>
    </return-response>
  </when>
</choose>

```

### <a name="elements"></a>Öğeler

| Öğe    | Açıklama   | Gerekli |
| ---------- | ------------- | -------- |
| set durumu | Kök öğesi. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik       | Açıklama                                                | Gerekli | Varsayılan |
| --------------- | ---------------------------------------------------------- | -------- | ------- |
| kod="insa"  | Döndürülecek HTTP durum kodu.                            | Evet      | Yok     |
| neden="string" | Durum kodunu döndürme nedeninin açıklaması. | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** giden, arka uç, on-error
-   **İlke kapsamları:** tüm kapsamlar

## <a name="set-variable"></a><a name="set-variable"></a>Değişkeni ayarlama

İlke `set-variable` bir [bağlam](api-management-policy-expressions.md#ContextVariables) değişkeni bildirir ve bir [ifade](api-management-policy-expressions.md) veya bir dize literal ile belirtilen bir değer atar. ifade bir edebi içeriyorsa bir dize dönüştürülür ve değer türü `System.String`olacaktır.

### <a name="policy-statement"></a><a name="set-variablePolicyStatement"></a>İlke bildirimi

```xml
<set-variable name="variable name" value="Expression | String literal" />
```

### <a name="example"></a><a name="set-variableExample"></a>Örnek

Aşağıdaki örnek, gelen bölümde bir dizi değişken ilkesini gösterir. Bu ayar değişken ilkesi, [context](api-management-policy-expressions.md#ContextVariables) istek `User-Agent` üstbilgisi metni `iPad` içeriyorsa `iPhone` `isMobile` veya .

```xml
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />
```

### <a name="elements"></a>Öğeler

| Öğe      | Açıklama   | Gerekli |
| ------------ | ------------- | -------- |
| set değişkeni | Kök öğesi. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik | Açıklama                                                              | Gerekli |
| --------- | ------------------------------------------------------------------------ | -------- |
| ad      | Değişkenin adı.                                                | Evet      |
| value     | Değişkenin değeri. Bu bir ifade veya gerçek bir değer olabilir. | Evet      |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error
-   **İlke kapsamları:** tüm kapsamlar

### <a name="allowed-types"></a><a name="set-variableAllowedTypes"></a>İzin verilen türler

İlkede `set-variable` kullanılan ifadeler aşağıdaki temel türlerden birini döndürmelidir.

-   System.Boolean
-   System.SByte
-   System.Byte
-   Sistem.UInt16
-   Sistem.UInt32
-   Sistem.UInt64
-   Sistem.Int16
-   Sistem.Int32
-   Sistem.Int64
-   Decimal
-   System.Single
-   Double
-   System.Guid
-   System
-   Char
-   Datetime
-   Timespan
-   System.Byte mı?
-   System.UInt16 mı?
-   System.UInt32?
-   System.UInt64?
-   System.Int16 mı?
-   System.Int32 mi?
-   System.Int64 mi?
-   Decimal?
-   System.Single mı?
-   Double?
-   System.Guid mi?
-   System?
-   Char?
-   Datetime?

## <a name="trace"></a><a name="Trace"></a>Izleme

İlke, `trace` API Denetçisi çıktısına, Application Insights telemetries'e ve/veya Tanıgünlüklerine özel bir izleme ekler.

-   İlke, izleme tetiklendiğinde [API Denetçisi](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) çıktısına özel bir `Ocp-Apim-Trace` izleme ekler, yani istek üstbilgisi vardır ve doğru olarak ayarlanır ve `Ocp-Apim-Subscription-Key` istek üstbilgisi bulunur ve izleme sağlayan geçerli bir anahtar tutar.
-   İlke, [Uygulama Öngörüleri tümleştirmesi](https://docs.microsoft.com/azure/api-management/api-management-howto-app-insights) etkinleştirildiğinde ve ilkede `severity` belirtilen düzey tanılama ayarında `verbosity` belirtilen düzeyde veya daha yüksek olduğunda, Uygulama Öngörüleri'nde bir [İzleme](https://docs.microsoft.com/azure/azure-monitor/app/data-model-trace-telemetry) telemetrisi oluşturur.
-   Tanılama [Günlükleri](https://docs.microsoft.com/azure/api-management/api-management-howto-use-azure-monitor#diagnostic-logs) etkinleştirildiğinde ve ilkede belirtilen önem düzeyi tanılama ayarında belirtilen ayrıntılılık düzeyinde veya daha yüksek olduğunda, ilke girişine bir özellik ekler.

### <a name="policy-statement"></a>İlke bildirimi

```xml

<trace source="arbitrary string literal" severity="verbose|information|error">
    <message>String literal or expressions</message>
    <metadata name="string literal or expressions" value="string literal or expressions"/>
</trace>

```

### <a name="example"></a><a name="traceExample"></a>Örnek

```xml
<trace source="PetStore API" severity="verbose">
    <message>@((string)context.Variables["clientConnectionID"])</message>
    <metadata name="Operation Name" value="New-Order"/>
</trace>
```

### <a name="elements"></a>Öğeler

| Öğe  | Açıklama                                                                                                                                          | Gerekli |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| izleme    | Kök öğesi.                                                                                                                                        | Evet      |
| message  | Günlüğe kaydedilecek bir dize veya ifade.                                                                                                                 | Evet      |
| meta veriler | Uygulama Öngörüleri [İzleme](https://docs.microsoft.com/azure/azure-monitor/app/data-model-trace-telemetry) telemetrisine özel bir özellik ekler. | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Öznitelik | Açıklama                                                                                                               | Gerekli | Varsayılan |
| --------- | ------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| source    | İzleyiciye anlamlı dize ve iletinin kaynağını belirtir.                                   | Evet      | Yok     |
| önem derecesi  | İzlemenin önem düzeyini belirtir. İzin verilen `verbose` `information`değerler `error` , , , (en düşükten en yükseğe) | Hayır       | Ayrıntılı |
| ad      | Özelliğin adı.                                                                                                     | Evet      | Yok     |
| value     | Özelliğin değeri.                                                                                                    | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

## <a name="wait"></a><a name="Wait"></a>Bekle

İlke, `wait` hemen alt ilkelerini paralel olarak yürütür ve tamamlanmadan önce tümünün veya bir acil alt ilkesinin tamamlanmasını bekler. Bekleme ilkesi, hemen alt ilkeleri olarak [istek gönder,](api-management-advanced-policies.md#SendRequest) [önbellekten değer alın](api-management-caching-policies.md#GetFromCacheByKey)ve akış ilkelerini [denetleyebilir.](api-management-advanced-policies.md#choose)

### <a name="policy-statement"></a>İlke bildirimi

```xml
<wait for="all|any">
  <!--Wait policy can contain send-request, cache-lookup-value,
        and choose policies as child elements -->
</wait>

```

### <a name="example"></a>Örnek

Aşağıdaki `choose` `wait` örnekte, ilkeğin hemen alt ilkeleri olarak iki ilke vardır. Bu `choose` ilkelerin her biri paralel olarak yürütülür. Her `choose` ilke önbelleğe alınmış bir değer almaya çalışır. Önbellek kaçığı varsa, değeri sağlamak için bir arka uç hizmeti çağrılır. Bu örnekte, `wait` `for` öznitelik . `all` Bu örnekte bağlam değişkenleri `value-one` `execute-branch-two`(`execute-branch-one` `value-two`, , , ve ) bu örnek ilkesinin kapsamı dışında beyan edilir.

```xml
<wait for="all">
  <choose>
    <when condition="@((bool)context.Variables["execute-branch-one="])">
      <cache-lookup-value key="key-one" variable-name="value-one" />
      <choose>
        <when condition="@(!context.Variables.ContainsKey("value-one="))">
          <send-request mode="new" response-variable-name="value-one">
            <set-url>https://backend-one</set-url>
            <set-method>GET</set-method>
          </send-request>
        </when>
      </choose>
    </when>
  </choose>
  <choose>
    <when condition="@((bool)context.Variables["execute-branch-two="])">
      <cache-lookup-value key="key-two" variable-name="value-two" />
      <choose>
        <when condition="@(!context.Variables.ContainsKey("value-two="))">
          <send-request mode="new" response-variable-name="value-two">
            <set-url>https://backend-two</set-url>
            <set-method>GET</set-method>
          </send-request>
        </when>
      </choose>
    </when>
  </choose>
</wait>

```

### <a name="elements"></a>Öğeler

| Öğe | Açıklama                                                                                                   | Gerekli |
| ------- | ------------------------------------------------------------------------------------------------------------- | -------- |
| Bekle    | Kök öğesi. Yalnızca `send-request` `cache-lookup-value`alt öğeler ve `choose` ilkeler içerebilir. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                            | Gerekli | Varsayılan |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| for       | İlkenin `wait` tüm acil alt ilkelerin tamamlanmasını mı yoksa sadece bir tanesini mi beklediğini belirler. İzin verilen değerler şunlardır:<br /><br /> - `all`- tüm acil çocuk politikalarının tamamlanmasını bekleyin<br />- herhangi bir - herhangi bir acil çocuk politikası tamamlamak için bekleyin. İlk acil alt ilke tamamlandıktan sonra, `wait` ilke tamamlanır ve diğer acil alt politikaların yürütülmesi sonlandırılır. | Hayır       | tümü     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç
-   **İlke kapsamları:** tüm kapsamlar

## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışan daha fazla bilgi için bkz:

-   [API Yönetiminde İlkeler](api-management-howto-policies.md)
-   [İlke ifadeleri](api-management-policy-expressions.md)
-   [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
-   [İlke örnekleri](policy-samples.md)
