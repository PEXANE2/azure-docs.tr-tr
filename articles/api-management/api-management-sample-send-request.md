---
title: HTTP isteklerini oluşturmak için API Yönetimi hizmetini kullanma
description: API'nizden harici hizmetleri aramak için API Yönetimi'nde istek ve yanıt ilkelerini kullanmayı öğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 4539c0fa-21ef-4b1c-a1d4-d89a38c242fa
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 1c86570850894a47f57a2d3587811411cc9a76eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190005"
---
# <a name="using-external-services-from-the-azure-api-management-service"></a>Azure API Yönetimi hizmetindeki harici hizmetleri kullanma
Azure API Yönetimi hizmetinde kullanılabilen ilkeler, yalnızca gelen istek, giden yanıt ve temel yapılandırma bilgilerine dayalı olarak çok çeşitli yararlı işler yapabilir. Ancak, API Yönetimi ilkelerinden dış hizmetlerle etkileşim kurabilmek çok daha fazla fırsat açar.

[Günlüğe kaydetme, izleme ve analiz için Azure Event Hub hizmetiyle](api-management-log-to-eventhub-sample.md)nasıl etkileşimkurabileceğinizi daha önce gördünüz. Bu makalede, herhangi bir dış HTTP tabanlı hizmet ile etkileşim sağlayan ilkeler gösterin. Bu ilkeler, uzak olayları tetiklemek veya özgün istek ve yanıtı bir şekilde işlemek için kullanılan bilgileri almak için kullanılabilir.

## <a name="send-one-way-request"></a>Gönder-Tek Yönlü-İstek
Muhtemelen en basit dış etkileşim, harici bir hizmetin bir tür önemli olaydan haberdar edilmesine olanak tanıyan yangın ve unut isteği tarzıdır. Denetim akışı `choose` ilkesi, ilgilendiğiniz her türlü koşulu algılamak için kullanılabilir.  Koşul karşılanırsa, [tek yönlü istek gönderme](/azure/api-management/api-management-advanced-policies#SendOneWayRequest) ilkesini kullanarak harici bir HTTP isteği nde bulunabilirsiniz. Bu Hipchat veya Slack gibi bir mesajlaşma sistemi için bir istek veya SendGrid veya MailChimp gibi bir posta API, ya da PagerDuty gibi kritik destek olayları bir şey olabilir. Tüm bu mesajlaşma sistemleri çağrılabilir basit HTTP API'ler var.

### <a name="alerting-with-slack"></a>Bolluk ile Uyarı
Aşağıdaki örnek, HTTP yanıt durum kodu 500'den büyük veya eşitse, Slack sohbet odasına nasıl ileti gönderilenleri gösterir. 500 aralığı hatası, API istemcisinin kendi başlarına çözemediği arka uç API'si ile ilgili bir sorunu gösterir. Genellikle API Yönetimi kısmında bir tür müdahale gerektirir.  

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

Slack gelen web kancaları kavramı vardır. Gelen bir web kancasını yapılandırırken, Slack basit bir POST isteği yapmanızı ve Slack kanalına bir ileti iletmenizi sağlayan özel bir URL oluşturur. Oluşturduğunuz JSON gövdesi, Slack tarafından tanımlanan bir biçime dayanır.

![Bolluk Web Hook](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Ateş ve unutmak yeterince iyi mi?
Bir yangın ve unut isteği stili ni kullanırken bazı dengeler vardır. Herhangi bir nedenle istek başarısız olursa, hata bildirilmez. Bu özel durumda, ikincil bir hata raporlama sistemine sahip olmanın karmaşıklığı ve yanıtı beklemenin ek performans maliyeti garanti edilmez. Yanıtı denetlemenin gerekli olduğu senaryolar [için, gönder-istek](/azure/api-management/api-management-advanced-policies#SendRequest) ilkesi daha iyi bir seçenektir.

## <a name="send-request"></a>Gönder-İstek
İlke, `send-request` karmaşık işleme işlevlerigerçekleştirmek ve verileri daha fazla ilke işleme için kullanılabilecek API yönetim hizmetine döndürmek için harici bir hizmet inemesini sağlar.

### <a name="authorizing-reference-tokens"></a>Referans belirteçlerini yetkilendirme
API Yönetimi'nin önemli bir işlevi arka uç kaynaklarını korumaktır. API'niz tarafından kullanılan yetkilendirme sunucusu, [Azure Active Directory'nin](../active-directory/hybrid/whatis-hybrid-identity.md) yaptığı gibi OAuth2 akışının bir parçası `validate-jwt` olarak [JWT belirteçleri](https://jwt.io/) oluşturuyorsa, belirtecinin geçerliliğini doğrulamak için ilkeyi kullanabilirsiniz. Bazı yetkilendirme sunucuları, yetkilendirme sunucusuna geri arama yapmadan doğrulanamayan [başvuru belirteçleri](https://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) olarak adlandırılan bir şey oluşturur.

### <a name="standardized-introspection"></a>Standart içgözlem
Geçmişte, bir yetkilendirme sunucusuyla bir başvuru belirteci doğrulamanın standart bir yolu olmamıştır. Ancak, bir kaynak sunucusunun bir belirteçgeçerliliğini nasıl doğrulayabileceğini tanımlayan IETF tarafından yakın zamanda önerilen standart [RFC 7662](https://tools.ietf.org/html/rfc7662) yayımlanmıştır.

### <a name="extracting-the-token"></a>Belirteci ayıklama
İlk adım, yetkilendirme üstbilgisinden belirteç ayıklamaktır. Üstbilgi değeri `Bearer` yetkilendirme düzeni, tek bir boşluk ve ardından [RFC 6750'ye](https://tools.ietf.org/html/rfc6750#section-2.1)göre yetkilendirme belirteciyle biçimlendirilmelidir. Ne yazık ki yetkilendirme şeması atlanır durumlar vardır. Ayrıştırma yaparken bunu hesaba katmak için, API Yönetimi üstbilgi değerini bir boşlukta böler ve döndürülen dize dizisinden son dizeyi seçer. Bu, kötü biçimlendirilmiş yetkilendirme üstbilgiiçin bir geçici çözüm sağlar.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a>Doğrulama isteği yapma
API Yönetimi yetkilendirme belirtecine sahip olduktan sonra, API Yönetimi belirteci doğrulamak için istekte bulunabilir. RFC 7662 bu işlem içgözlem çağırır `POST` ve içgözlem kaynağı için bir HTML formu gerektirir. HTML formu en azından anahtarla `token`birlikte bir anahtar/değer çifti içermelidir. Kötü amaçlı istemcilerin geçerli jetonlar için trol leğen ize gidemediğinden emin olmak için yetkilendirme sunucusuna yapılan bu isteğin de doğrulanması gerekir.

```xml
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
```

### <a name="checking-the-response"></a>Yanıtı denetleme
Öznitelik, `response-variable-name` döndürülen yanıta erişim vermek için kullanılır. Bu özellikte tanımlanan ad, `context.Variables` `IResponse` nesneye erişmek için sözlükte anahtar olarak kullanılabilir.

Yanıt nesnesinden gövdeyi alabilirsiniz ve RFC 7622 API Management'a yanıtın bir JSON nesnesi olması gerektiğini ve en azından boolean değeri olan `active` bir özellik içermesi gerektiğini söyler. Doğru `active` olduğunda belirteç geçerli kabul edilir.

Alternatif olarak, yetkilendirme sunucusu belirteç geçerli olup olmadığını belirtmek için "etkin" alanını içermiyorsa, geçerli bir belirteçte hangi özelliklerin ayarlanıp ayarlmadığını belirlemek için Postacı gibi bir araç kullanın. Örneğin, geçerli bir belirteç yanıtı "expires_in" adlı bir özellik içeriyorsa, bu özellik adının yetkilendirme sunucusu yanıtında bu şekilde bulunup bulunmadığından denetleyin:

<zaman koşul="@(((IResponse)bağlam. Değişkenler["belirteç durumu"]). Body.As<JObject>(). Özellik("expires_in") == null)">

### <a name="reporting-failure"></a>Raporlama hatası
Belirteç `<choose>` geçersiz olup olmadığını algılamak için bir ilke kullanabilirsiniz ve öyleyse, bir 401 yanıtı döndürün.

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

Belirteçlerin nasıl `bearer` kullanılması gerektiğini açıklayan [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) uyarınca, `WWW-Authenticate` API Yönetimi de 401 yanıtıyla bir üstbilgi döndürür. WWW-Authenticate, istemciye doğru yetkilendirilen bir isteğin nasıl oluşturulabildiğini öğretmek için tasarlanmıştır. OAuth2 çerçevesi ile mümkün olan çok çeşitli yaklaşımlar nedeniyle, gerekli tüm bilgileri iletmek zordur. Neyse ki [istemciler düzgün bir kaynak sunucusuna istekleri ni nasıl yetki](https://tools.ietf.org/html/draft-jones-oauth-discovery-00)keşfetmek yardımcı olmak için devam çabaları vardır.

### <a name="final-solution"></a>Nihai çözüm
Sonunda, aşağıdaki ilkeyi alırsınız:

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
              <return-response response-variable-name="existing response variable">
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

Bu, ilkenin `send-request` yararlı dış hizmetleri API Yönetimi hizmeti aracılığıyla akan istek ve yanıtlar sürecine entegre etmek için nasıl kullanılabileceğinigösteren örneklerden yalnızca biridir.

## <a name="response-composition"></a>Yanıt Kompozisyonu
İlke, `send-request` önceki örnekte gördüğünüz gibi, birincil isteği arka uç sistemine geliştirmek için kullanılabilir veya arka uç aramasının tam bir yerine kullanılabilir. Bu tekniği kullanarak kolayca birden çok farklı sistemlerden toplanan bileşik kaynakları oluşturabilirsiniz.

### <a name="building-a-dashboard"></a>Pano oluşturma
Bazen, örneğin bir panoyu kullanmak için birden çok arka uç sisteminde bulunan bilgileri ortaya çıkarmak istersiniz. KP'ler tüm farklı arka uçlardan gelir, ancak bunlara doğrudan erişim sağlamamayı tercih edersiniz ve tüm bilgilerin tek bir istekte alınabilmesi güzel olurdu. Belki bazı arka uç bilgileri bazı dilimleme ve doğrama ve biraz önce sanitizing ihtiyacı var! Kullanıcıların düşük performans lı ölçümlerinin değişip değişmediğini görmek için F5 tuşunu çekiçleme alışkanlığı olduğunu bildiğiniz için, bu bileşik kaynağın önbelleği kullanılabilir.    

### <a name="faking-the-resource"></a>Kaynağı niçin taklit etme
Pano kaynağını oluşturmanın ilk adımı, Azure portalında yeni bir işlemi yapılandırmaktır. Bu, dinamik kaynağı oluşturmak için kompozisyon ilkesini yapılandırmak için kullanılan bir yer tutucu işlemidir.

![Pano çalışması](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>İstekleri yapma
İşlem oluşturulduktan sonra, bu işlem için özel olarak bir ilke yapılandırabilirsiniz. 

![Pano çalışması](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

İlk adım, gelen istekten herhangi bir sorgu parametrelerini ayıklamaktır, böylece bunları arka uca iletebilirsiniz. Bu örnekte, pano bilgileri bir zaman dönemine göre `fromDate` gösterir `toDate` ve bu nedenle bir ve parametresi vardır. İlkeyi, `set-variable` bilgileri istek URL'sinden ayıklamak için kullanabilirsiniz.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

Bu bilgilere sahip olduktan sonra, tüm arka uç sistemlerine istekte bulunabilirsiniz. Her istek parametre bilgilerini içeren yeni bir URL oluşturuyor ve yanıtı ilgili sunucuyu çağırır ve yanıtı bir bağlam değişkeninde depolar.

```xml
<send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
  <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
  <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/accidentdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>
```

Bu istekler, ideal olmayan sırayla yürütülür. 

### <a name="responding"></a>Yanıt
Bileşik yanıtı oluşturmak için iade [yanıt](/azure/api-management/api-management-advanced-policies#ReturnResponse) ilkesini kullanabilirsiniz. Öğe, `set-body` tüm bileşen gösterimleri `JObject` özellikleri olarak gömülü yeni bir oluşturmak için bir ifade kullanabilirsiniz.

```xml
<return-response response-variable-name="existing response variable">
  <set-status code="200" reason="OK" />
  <set-header name="Content-Type" exists-action="override">
    <value>application/json</value>
  </set-header>
  <set-body>
    @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                  new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                  new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                  new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                  ).ToString())
  </set-body>
</return-response>
```

Tam ilke aşağıdaki gibi görünür:

```xml
<policies>
    <inbound>

  <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
  <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/accidentdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
</policies>
```

Yer tutucu işleminin yapılandırmasında, pano kaynağını en az bir saat önbelleğe alınacak şekilde yapılandırabilirsiniz. 

## <a name="summary"></a>Özet
Azure API Yönetimi hizmeti, HTTP trafiğine seçkiyle uygulanabilen esnek ilkeler sağlar ve arka uç hizmetlerinin birleşimini sağlar. İster uyarı işlevleri, doğrulama, doğrulama özellikleriyle API ağ geçidinizi geliştirmek ister birden çok arka `send-request` uç hizmetine dayalı yeni bileşik kaynaklar oluşturmak istiyorsanız, ilgili ilkeler olasılıklar dünyasını açar.

