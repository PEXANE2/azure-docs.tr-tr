---
title: Azure API Yönetimi dönüşüm ilkeleri | Microsoft Dokümanlar
description: Azure API Yönetimi'nde kullanılabilir dönüşüm ilkeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/11/2019
ms.author: apimpm
ms.openlocfilehash: 81b7fb687bb6ef88d1ed436923d0e5ff7561c22b
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80803254"
---
# <a name="api-management-transformation-policies"></a>API Management dönüştürme ilkeleri
Bu konu, aşağıdaki API Yönetimi ilkeleri için bir başvuru sağlar. İlkeler ekleme ve yapılandırma hakkında bilgi için [API Yönetimi'ndeki İlkeler'e](https://go.microsoft.com/fwlink/?LinkID=398186)bakın.

##  <a name="transformation-policies"></a><a name="TransformationPolicies"></a>Dönüşüm ilkeleri

-   [JSON'u XML'e dönüştürün](api-management-transformation-policies.md#ConvertJSONtoXML) - İstek veya yanıt gövdesini JSON'dan XML'e dönüştürür.

-   [XML'i JSON'a dönüştürün](api-management-transformation-policies.md#ConvertXMLtoJSON) - İstek veya yanıt gövdesini XML'den JSON'a dönüştürür.

-   [Gövdedeki dizeyi bul ve değiştir](api-management-transformation-policies.md#Findandreplacestringinbody) - Bir istek veya yanıt alt dizesi bulur ve farklı bir alt dize ile değiştirir.

-   [İçerikteki URL'leri maskeleyin](api-management-transformation-policies.md#MaskURLSContent) - Yanıt gövdesindeki bağlantıları yeniden yazar (maskeler) böylece ağ geçidi üzerinden eşdeğer bağlantıyı işaret ederler.

-   [Arka uç hizmetini ayarlama](api-management-transformation-policies.md#SetBackendService) - Gelen bir istek için arka uç hizmetini değiştirir.

-   [Gövdeyi ayarla](api-management-transformation-policies.md#SetBody) - Gelen ve giden istekler için ileti gövdesini ayarlar.

-   [HTTP üstbilgi sini ayarlayın](api-management-transformation-policies.md#SetHTTPheader) - Varolan bir yanıta ve/veya istek üstbilgisine bir değer atar veya yeni bir yanıt ve/veya istek üstbilgisi ekler.

-   [Sorgu dize parametresini ayarlayın](api-management-transformation-policies.md#SetQueryStringParameter) - İstek sorgusu dize parametresinin değerini ekler, değiştirir veya siler.

-   [URL'yi Yeniden Yazma](api-management-transformation-policies.md#RewriteURL) - İstek URL'sini genel formundan web hizmetinin beklediği forma dönüştürür.

-   [XSLT kullanarak XML'i dönüştürün](api-management-transformation-policies.md#XSLTransform) - İstek veya yanıt gövdesinde XML'ye XSL dönüşümü uygular.

##  <a name="convert-json-to-xml"></a><a name="ConvertJSONtoXML"></a>JSON'u XML'e dönüştürün
 İlke, `json-to-xml` bir istek veya yanıt gövdesini JSON'dan XML'e dönüştürür.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false" parse-date="true | false"/>
```

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <outbound>
        <base />
        <json-to-xml apply="always" consider-accept-header="false" parse-date="false"/>
    </outbound>
</policies>
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|json-to-xml|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|apply|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> - Her zaman - her zaman dönüşüm uygulayın.<br />- content-type-json - yalnızca yanıt İçerik-Türü üstbilgi JSON varlığını gösterirse dönüştürün.|Evet|Yok|
|consider-accept-header|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> - true - istek üstbilgisinde XML isteniyorsa dönüştürme uygulayın.<br />- yanlış -her zaman dönüştürme uygulayın.|Hayır|true|
|ayrışdırış tarihi|Bugüne kadar `false` ayarlanan değerler dönüşüm sırasında yalnızca kopyalandığında|Hayır|true|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, on-error

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="convert-xml-to-json"></a><a name="ConvertXMLtoJSON"></a>XML'i JSON'a dönüştürün
 İlke, `xml-to-json` bir istek veya yanıt gövdesini XML'den JSON'a dönüştürür. Bu ilke, Yalnızca XML arka uç web hizmetlerini temel alan API'leri modernize etmek için kullanılabilir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>
```

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <outbound>
        <base />
        <xml-to-json kind="direct" apply="always" consider-accept-header="false" />
    </outbound>
</policies>
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|xml-to-json|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|Tür|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> - javascript dostu - dönüştürülmüş JSON JavaScript geliştiricileri için dostu bir formu vardır.<br />- doğrudan - dönüştürülmüş JSON orijinal XML belgenin yapısını yansıtır.|Evet|Yok|
|apply|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> - her zaman - her zaman dönüştürmek.<br />- content-type-xml - yalnızca yanıt İçerik-Türü üstbilgi XML varlığını gösterirse dönüştürün.|Evet|Yok|
|consider-accept-header|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> - doğru - JSON istek üstbilgikabul üstbilgisinde istenirse dönüşüm uygulayın.<br />- yanlış -her zaman dönüştürme uygulayın.|Hayır|true|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, on-error

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="find-and-replace-string-in-body"></a><a name="Findandreplacestringinbody"></a>Vücuttaki dizeyi bulma ve değiştirme
 İlke `find-and-replace` bir istek veya yanıt alt dizesini bulur ve farklı bir alt dize ile değiştirir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<find-and-replace from="what to replace" to="replacement" />
```

### <a name="example"></a>Örnek

```xml
<find-and-replace from="notebook" to="laptop" />
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|bul-ve-değiştir|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|Kaynak|Aranacak dize.|Evet|Yok|
|-|Değişim dizesi. Arama dizesini kaldırmak için sıfır uzunluk değiştirme dizesi belirtin.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="mask-urls-in-content"></a><a name="MaskURLSContent"></a>İçerikteki URL'leri maskeleyin
 İlke, `redirect-content-urls` yanıt gövdesindeki bağlantıları yeniden yazar (maskeler) böylece ağ geçidi üzerinden eşdeğer bağlantıyı işaret eder. Yanıt gövdesi bağlantı larını yeniden yazmak için giden bölümde kullanın ve ağ geçidini işaret edin. Ters etki için gelen bölümde kullanın.

> [!NOTE]
>  Bu ilke, üstbilgi gibi `Location` üstbilgi değerlerini değiştirmez. Üstbilgi değerlerini değiştirmek [için, üstbilgi ilkesini](api-management-transformation-policies.md#SetHTTPheader) kullanın.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<redirect-content-urls />
```

### <a name="example"></a>Örnek

```xml
<redirect-content-urls />
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|yönlendirme-içerik-url'leri|Kök öğesi.|Evet|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="set-backend-service"></a><a name="SetBackendService"></a>Arka uç hizmetini ayarlama
 Gelen `set-backend-service` isteği, söz tür işlemi için API ayarlarında belirtilenden farklı bir arka uca yönlendirmek için ilkeyi kullanın. Bu ilke, gelen isteğin arka uç hizmet temel URL'sini ilkede belirtilen etekte belirtilene değiştirir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-backend-service base-url="base URL of the backend service" />
```

or

```xml
<set-backend-service backend-id="identifier of the backend entity specifying base URL of the backend service" />
```

> [!NOTE]
> Arka uç varlıklar yönetim [API](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/backend) ve [PowerShell](https://www.powershellgallery.com/packages?q=apimanagement)üzerinden yönetilebilir.

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <choose>
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2013-05")">
                <set-backend-service base-url="http://contoso.com/api/8.2/" />
            </when>
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2014-03")">
                <set-backend-service base-url="http://contoso.com/api/9.1/" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```
Bu örnekte, sorgu dizesinde geçirilen sürüm değerini temel alan set backend servis ilkesi isteklerini API'de belirtilenden farklı bir arka uç hizmetine yönlendirir.

Başlangıçta arka uç hizmet tabanı URL API ayarlarından türetilmiştir. Böylece istek `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` URL'si, API ayarlarında belirtilen arka uç hizmet URL'si nerede `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` `http://contoso.com/api/10.4/` olur.

<[ilke\> ](api-management-advanced-policies.md#choose) bildirimi ni seçtiğinde, sürüm isteği sorgu `http://contoso.com/api/8.2` parametresinin değerine bağlı olarak arka uç hizmet tabanı URL'si yeniden `http://contoso.com/api/9.1`değiştirilebilir. Örneğin, değer son `"2013-15"` istek URL'si ise `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.

İsteğin daha fazla dönüştürülmesi isteniyorsa, diğer [Dönüşüm ilkeleri](api-management-transformation-policies.md#TransformationPolicies) kullanılabilir. Örneğin, istek belirli bir arka uça yönlendirilirken sürüm sorgu parametresini kaldırmak için, artık yedek sürüm özniteliğini kaldırmak için [Sorgu dizesi parametre](api-management-transformation-policies.md#SetQueryStringParameter) ilkesi kullanılabilir.

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <set-backend-service backend-id="my-sf-service" sf-partition-key="@(context.Request.Url.Query.GetValueOrDefault("userId","")" sf-replica-type="primary" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```
Bu örnekte ilke, kullanıcıKimliği sorgu dizesini bölüm anahtarı olarak kullanarak ve bölümün birincil yinelemesini kullanarak isteği bir hizmet dokusu arka ucuna yönlendirir.

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|set-backend-service|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|taban-url|Yeni arka uç hizmet tabanı URL'si.|Biri `base-url` var `backend-id` olmalı ya da orada olmalı.|Yok|
|arka uç kimliği|Arka uçtan yola giden tanımlayıcı. (Arka uç varlıkları [API](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/backend) ve [PowerShell](https://www.powershellgallery.com/packages?q=apimanagement)üzerinden yönetilir.)|Biri `base-url` var `backend-id` olmalı ya da orada olmalı.|Yok|
|sf-partition-key|Yalnızca arka uç Bir Service Fabric hizmeti olduğunda ve 'backend-id' kullanılarak belirtildiğinde uygulanabilir. Ad çözümleme hizmetinden belirli bir bölümü çözmek için kullanılır.|Hayır|Yok|
|sf-çoğaltma türü|Yalnızca arka uç Bir Service Fabric hizmeti olduğunda ve 'backend-id' kullanılarak belirtildiğinde uygulanabilir. İsteğin bir bölümün birincil veya ikincil yinelemesine gitmesi gerekiyorsa denetimleri. |Hayır|Yok|
|sf-çözüm-durum|Yalnızca arka uç Service Fabric hizmeti olduğunda geçerlidir. Service Fabric backend'e yapılan çağrının yeni çözünürlükle tekrarlanması gerekip gerekmediğini belirleyen durum.|Hayır|Yok|
|sf-hizmet-örnek-isim|Yalnızca arka uç Service Fabric hizmeti olduğunda geçerlidir. Çalışma zamanında hizmet örneklerini değiştirmenize izin verir. |Hayır|Yok|
|sf-dinleyici-adı|Yalnızca arka uç Bir Service Fabric hizmeti olduğunda ve 'backend-id' kullanılarak belirtildiğinde uygulanabilir. Service Fabric Reliable Services, bir hizmette birden fazla dinleyici oluşturmanıza olanak tanır. Bu öznitelik, bir arka uç Güvenilir Hizmet birden fazla dinleyici yesahip olduğunda belirli bir dinleyici seçmek için kullanılır. Bu öznitelik belirtilmemişse, API Yönetimi bir ad olmadan bir dinleyici kullanmaya çalışır. Adı olmayan bir dinleyici, yalnızca bir dinleyicisi olan Güvenilir Hizmetler için tipiktir. |Hayır|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, arka uç

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="set-body"></a><a name="SetBody"></a>Gövdeyi ayarlama
 Gelen `set-body` ve giden istekler için ileti gövdesini ayarlamak için ilkeyi kullanın. İleti gövdesine erişmek için, `context.Request.Body` ilkenin gelen veya giden bölümde olup olmadığına bağlı olarak özelliği veya `context.Response.Body`özelliği kullanabilirsiniz.

> [!IMPORTANT]
>  İleti gövdesine kullanarak veya `context.Request.Body` `context.Response.Body`, özgün ileti gövdesini kullanarak erişdiğinizde varsayılan olarak kaybolduğunu ve gövdeyi ifadede geri döndürerek ayarlanması gerektiğini unutmayın. Gövde içeriğini korumak için, `preserveContent` parametreyi iletiye `true` erişirken ayarlayın. İfade `preserveContent` ile `true` farklı bir gövde ayarlanırsa ve farklı bir gövde döndürülürse, döndürülen gövde kullanılır.
>
>  Politikayı kullanırken lütfen aşağıdaki `set-body` hususlara dikkat ediniz.
>
> - Yeni veya güncelleştirilmiş bir gövdeyi döndürmek için `set-body` ilkeyi `preserveContent` kullanıyorsanız, yeni gövde içeriğini açıkça sağladığınız için ayarlamanız `true` gerekmez.
>   -   Gelen ardışık ardışık ardışık ardışık ardışık bir yanıtın içeriğini korumak mantıklı değildir, çünkü henüz yanıt yoktur.
>   -   Giden ardışık ardışık ardışık ardışık bir istek içeriğini korumak mantıklı değildir, çünkü istek bu noktada zaten arka uca gönderilmiştir.
>   -   Bu ilke, ileti gövdesi olmadığında kullanılırsa, örneğin gelen GET'de bir özel durum atılır.

 Daha fazla bilgi `context.Request.Body`için `context.Response.Body`Bağlam `IMessage` [değişken](api-management-policy-expressions.md#ContextVariables) tablosundaki , ve bölümlere bakın.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-body>new body value as text</set-body>
```

### <a name="examples"></a>Örnekler

#### <a name="literal-text-example"></a>Gerçek metin örneği

```xml
<set-body>Hello world!</set-body>
```

#### <a name="example-accessing-the-body-as-a-string-note-that-we-are-preserving-the-original-request-body-so-that-we-can-access-it-later-in-the-pipeline"></a>Örnek bir dize olarak vücuda erişme. Orijinal istek gövdesini koruduğumuza dikkat edin, böylece daha sonra boru hattında erişebilebilebiliyoruz.

```xml
<set-body>
@{ 
    string inBody = context.Request.Body.As<string>(preserveContent: true); 
    if (inBody[0] =='c') { 
        inBody[0] = 'm'; 
    } 
    return inBody; 
}
</set-body>
```

#### <a name="example-accessing-the-body-as-a-jobject-note-that-since-we-are-not-reserving-the-original-request-body-accessing-it-later-in-the-pipeline-will-result-in-an-exception"></a>JObject olarak gövdeye erişen örnek. Orijinal istek gövdesini rezerve etmediğimiz için, daha sonra ardışık hatlar'da bu gövdeye erişmenin bir özel durumla sonuçlanacaktır.

```xml
<set-body> 
@{ 
    JObject inBody = context.Request.Body.As<JObject>(); 
    if (inBody.attribute == <tag>) { 
        inBody[0] = 'm'; 
    } 
    return inBody.ToString(); 
} 
</set-body>

```

#### <a name="filter-response-based-on-product"></a>Ürüne dayalı filtre yanıtı
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

### <a name="using-liquid-templates-with-set-body"></a>Set gövdesi ile Sıvı şablonları kullanma
İlke, `set-body` bir istek veya yanıtın gövdesini dönüştürmek için [Sıvı](https://shopify.github.io/liquid/basics/introduction/) ayarlı dili kullanacak şekilde yapılandırılabilir. İletinizin biçimini tamamen yeniden şekillendirmeniz gerekiyorsa, bu çok etkili olabilir.

> [!IMPORTANT]
> İlkede `set-body` kullanılan Sıvı uygulaması 'C# modunda yapılandırılır. Bu, filtreleme gibi şeyler yaparken özellikle önemlidir. Örnek olarak, tarih filtresi kullanmak Pascal kasası ve C# tarih biçimlendirmesini gerektirir:
>
> {{body.foo.startDateTime| Tarih:"yyyyMddTHH:mm:ddZ"}}

> [!IMPORTANT]
> Sıvı şablonu kullanarak bir XML gövdesine doğru şekilde `set-header` bağlanabilmek için, Content-Type'ı uygulama/xml, metin/xml (veya +xml ile biten herhangi bir türe) ayarlamak için bir ilke kullanın; Bir JSON gövdesi için uygulama/json, metin/json (veya +json ile biten herhangi bir tür) olmalıdır.

#### <a name="convert-json-to-soap-using-a-liquid-template"></a>Sıvı şablonu kullanarak JSON'u SOAP'a dönüştürün
```xml
<set-body template="liquid">
    <soap:Envelope xmlns="http://tempuri.org/" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
        <soap:Body>
            <GetOpenOrders>
                <cust>{{body.getOpenOrders.cust}}</cust>
            </GetOpenOrders>
        </soap:Body>
    </soap:Envelope>
</set-body>
```

#### <a name="transform-json-using-a-liquid-template"></a>Sıvı şablonu kullanarak JSON'u dönüştürme
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|set gövdesi|Kök öğesi. Gövde metnini veya gövdeyi döndüren ifadeleri içerir.|Evet|

### <a name="properties"></a>Özellikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|şablon|Ayarlanan gövde ilkesinin çalışacağı templating modunu değiştirmek için kullanılır. Şu anda desteklenen tek değer:<br /><br />- sıvı - set gövde politikası sıvı templating motoru kullanacak |Hayır||

İstek ve yanıt la ilgili bilgilere erişmek için Sıvı şablonu aşağıdaki özelliklere sahip bir bağlam nesnesine bağlanabilir: <br />
<pre>context.
    Request.
        Url
        Method
        OriginalMethod
        OriginalUrl
        IpAddress
        MatchedParameters
        HasBody
        ClientCertificates
        Headers

    Response.
        StatusCode
        Method
        Headers
Url.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString

OriginalUrl.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString
</pre>



### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="set-http-header"></a><a name="SetHTTPheader"></a>HTTP üstbilgi
 İlke, `set-header` varolan bir yanıta ve/veya istek üstbilgisine bir değer atar veya yeni bir yanıt ve/veya istek üstbilgisi ekler.

 HTTP iletisine HTTP üstbilgilerinin listesini ekler. Gelen bir ardışık boru hattına yerleştirildiğinde, bu ilke, istek hedef hizmete geçirilen için HTTP üstbilgisini ayarlar. Giden bir ardışık boru hattına yerleştirildiğinde, bu ilke, ağ geçidinin istemcisine gönderilen yanıt için HTTP üstbilgisini ayarlar.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-header name="header name" exists-action="override | skip | append | delete">
    <value>value</value> <!--for multiple headers with the same name add additional value elements-->
</set-header>
```

### <a name="examples"></a>Örnekler

#### <a name="example---adding-header-override-existing"></a>Örnek - üstbilgi ekleme, varolan geçersiz kılma

```xml
<set-header name="some header name" exists-action="override">
    <value>20</value>
</set-header>
```
#### <a name="example---removing-header"></a>Örnek - üstbilgi kaldırma

```xml
 <set-header name="some header name" exists-action="delete" />
```



#### <a name="forward-context-information-to-the-backend-service"></a>Bağlam bilgilerini arka uç hizmetine iletme
 Bu örnek, bağlam bilgilerini arka uç hizmetine sağlamak için API düzeyinde ilkenin nasıl uygulanacağı gösterilmektedir. Bu politikayı yapılandırma ve kullanma nın bir gösterimi [için, Vlad Vinogradsky ile Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 10:30'a kadar ileri sarma bölümüne bakın. Saat 12:10'da, iş yerinde politikayı görebileceğiniz geliştirici portalında bir işlemi arama nın bir demosu vardır.

```xml
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->
<set-header name="x-request-context-data" exists-action="override">
  <value>@(context.User.Id)</value>
  <value>@(context.Deployment.Region)</value>
</set-header>
```

 Daha fazla bilgi için [İlke ifadeleri](api-management-policy-expressions.md) ve [Bağlam değişkenine](api-management-policy-expressions.md#ContextVariables)bakın.

> [!NOTE]
> Bir üstbilginin birden çok değeri bir CSV dizesine sıkıştırılır, örneğin:`headerName: value1,value2,value3`
>
> Özel durumlar, aşağıdakideğerleri içeren standartlaştırılmış üstbilgi içerir:
> - virgül içerebilir`User-Agent`( `WWW-Authenticate` `Proxy-Authenticate`, , ,
> - tarih içerebilir`Cookie`( `Set-Cookie` `Warning`, , ,
> - tarih (`Date` `Expires`, `If-Modified-Since` `If-Unmodified-Since`, `Last-Modified` `Retry-After`, , , , ) içerir.
>
> Bu özel durumlar söz konusu olduğunda, birden çok üstbilgi değeri tek bir dize içine sıkıştırılacak ve örneğin ayrı üstbilgi olarak geçirilecektir:`User-Agent: value1`
>`User-Agent: value2`
>`User-Agent: value3`

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|set-üstbilgi|Kök öğesi.|Evet|
|value|Ayarlanacak üst bilginin değerini belirtir. Aynı ada sahip birden çok `value` üstbilgi için ek öğeler ekleyin.|Hayır|

### <a name="properties"></a>Özellikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|var-eylem|Üstbilgi zaten belirtildiğinde hangi eylemin ne zaman gerçekleşip gerçekleşsin diye belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> - geçersiz kılma - varolan üstbilginin değerini değiştirir.<br />- atla - varolan üstbilgi değerinin yerini almaz.<br />- ek - varolan üstbilgi değerine değer ekler.<br />- sil - üstbilgi isteği kaldırır.<br /><br /> Aynı ada sahip birden çok giriş kaydedilecek `override` şekilde ayarlandığında, üstbilginin tüm girişlere göre ayarlandığı (birden çok kez listelenecek); yalnızca listelenen değerler sonuç olarak ayarlanır.|Hayır|override|
|ad|Ayarlanacak üstbilginin adını belirtir.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden, arka uç, on-error

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="set-query-string-parameter"></a><a name="SetQueryStringParameter"></a>Sorgu dize parametresi ayarlama
 İlke, `set-query-parameter` istek sorgusu dize parametresinin değerini ekler, değiştirir veya siler. İsteğe bağlı veya istekte hiç bulunmayan arka uç hizmeti tarafından beklenen sorgu parametrelerini geçirmek için kullanılabilir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-query-parameter name="param name" exists-action="override | skip | append | delete">
    <value>value</value> <!--for multiple parameters with the same name add additional value elements-->
</set-query-parameter>
```

#### <a name="example"></a>Örnek

```xml

<set-query-parameter name="api-key" exists-action="skip">
  <value>12345678901</value>
</set-query-parameter>

```

#### <a name="forward-context-information-to-the-backend-service"></a>Bağlam bilgilerini arka uç hizmetine iletme
 Bu örnek, bağlam bilgilerini arka uç hizmetine sağlamak için API düzeyinde ilkenin nasıl uygulanacağı gösterilmektedir. Bu politikayı yapılandırma ve kullanma nın bir gösterimi [için, Vlad Vinogradsky ile Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 10:30'a kadar ileri sarma bölümüne bakın. Saat 12:10'da, iş yerinde politikayı görebileceğiniz geliştirici portalında bir işlemi arama nın bir demosu vardır.

```xml
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->
<set-query-parameter name="x-product-name" exists-action="override">
  <value>@(context.Product.Name)</value>
</set-query-parameter>

```

 Daha fazla bilgi için [İlke ifadeleri](api-management-policy-expressions.md) ve [Bağlam değişkenine](api-management-policy-expressions.md#ContextVariables)bakın.

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|ayar-sorgu-parametresi|Kök öğesi.|Evet|
|value|Ayarlanacak sorgu parametresi değerini belirtir. Aynı ada sahip birden çok `value` sorgu parametresi için ek öğeler ekleyin.|Evet|

### <a name="properties"></a>Özellikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|var-eylem|Sorgu parametresi önceden belirtildiğinde gerçekleştirilecek eylemi belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> - geçersiz kılma - varolan parametrenin değerini değiştirir.<br />- atla - varolan sorgu parametre değerini değiştirmez.<br />- ek - varolan sorgu parametre değerine değer ekler.<br />- sil - sorgu parametresini istekten kaldırır.<br /><br /> Sorgu parametresi tüm girişlere göre ayarlanan aynı adla birden çok giriş kaydolmak üzere `override` ayarlandığında (birden çok kez listelenecek); yalnızca listelenen değerler sonuç olarak ayarlanır.|Hayır|override|
|ad|Ayarlanacak sorgu parametresinin adını belirtir.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, arka uç

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="rewrite-url"></a><a name="RewriteURL"></a>URL'yi Yeniden Yazma
 İlke, `rewrite-uri` bir istek URL'sini genel formundan aşağıdaki örnekte gösterildiği gibi web hizmeti tarafından beklenen forma dönüştürür.

- Genel URL -`http://api.example.com/storenumber/ordernumber`

- İstek URL ' i -`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`

  Bu ilke, insan ve/veya tarayıcı dostu bir URL'nin web hizmeti tarafından beklenen URL biçimine dönüştürülmesi gerektiğinde kullanılabilir. Bu ilenin yalnızca temiz URL'ler, YENI lendirilebilir URL'ler, kullanıcı dostu URL'ler veya sorgu dizesi içermeyen ve bunun yerine yalnızca kaynağın yolunu içeren (şema ve yetki) içeren tamamen yapısal URL'ler gibi alternatif bir URL biçimi ortaya çıkarırken uygulanması gerekir. Bu genellikle estetik, kullanılabilirlik veya arama motoru optimizasyonu (SEO) amaçlar için yapılır.

> [!NOTE]
>  Yalnızca ilkeyi kullanarak sorgu dize parametreleri ekleyebilirsiniz. Yeniden yazma URL'sinde ek şablon yolu parametreleri ekleyemezsiniz.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />
```

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
        <rewrite-uri template="/v2/US/hardware/{storenumber}&{ordernumber}?City=city&State=state" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>
    <inbound>
        <base />
        <rewrite-uri template="/put" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
<!-- Resulting URL will be /put?c=d -->
```
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>
    <inbound>
        <base />
        <rewrite-uri template="/put" copy-unmatched-params="false" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
<!-- Resulting URL will be /put -->
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|yeniden yazma-uri|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Öznitelik|Açıklama|Gerekli|Varsayılan|
|---------------|-----------------|--------------|-------------|
|şablon|Herhangi bir sorgu dize parametreleri ile gerçek web hizmeti URL.. İfadeleri kullanırken, tüm değer bir ifade olmalıdır.|Evet|Yok|
|kopya-eşleşmez-paramlar|Gelen istekteki sorgu parametrelerinin özgün URL şablonunda bulunup bulunmadığını yeniden yazma şablonu tarafından tanımlanan URL'ye eklenip eklenmediğini belirtir|Hayır|true|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen

-   **İlke kapsamları:** tüm kapsamlar

##  <a name="transform-xml-using-an-xslt"></a><a name="XSLTransform"></a>XSLT kullanarak XML'i dönüştürün
 İlke, `Transform XML using an XSLT` istek veya yanıt gövdesinde XML'ye xsl dönüşümü uygular.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<xsl-transform>
    <parameter name="User-Agent">@(context.Request.Headers.GetValueOrDefault("User-Agent","non-specified"))</parameter>
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
        <xsl:output method="xml" indent="yes" />
        <xsl:param name="User-Agent" />
        <xsl:template match="* | @* | node()">
            <xsl:copy>
                <xsl:if test="self::* and not(parent::*)">
                    <xsl:attribute name="User-Agent">
                        <xsl:value-of select="$User-Agent" />
                    </xsl:attribute>
                </xsl:if>
                <xsl:apply-templates select="* | @* | node()" />
            </xsl:copy>
        </xsl:template>
    </xsl:stylesheet>
  </xsl-transform>
```

### <a name="example"></a>Örnek

```xml
<policies>
  <inbound>
      <base />
  </inbound>
  <outbound>
      <base />
      <xsl-transform>
        <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
            <xsl:output omit-xml-declaration="yes" method="xml" indent="yes" />
            <!-- Copy all nodes directly-->
            <xsl:template match="node()| @*|*">
                <xsl:copy>
                    <xsl:apply-templates select="@* | node()|*" />
                </xsl:copy>
            </xsl:template>
        </xsl:stylesheet>
    </xsl-transform>
  </outbound>
</policies>
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|xsl dönüşümü|Kök öğesi.|Evet|
|parametre|Dönüştürmede kullanılan değişkenleri tanımlamak için kullanılır|Hayır|
|xsl:stylesheet|Kök stil sayfası öğesi. Standart [XSLT belirtiminde](https://www.w3.org/TR/xslt) tanımlanan tüm öğeler ve öznitelikler|Evet|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden

-   **İlke kapsamları:** tüm kapsamlar

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

+ [API Yönetiminde İlkeler](api-management-howto-policies.md)
+ [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
+ [İlke örnekleri](policy-samples.md)
