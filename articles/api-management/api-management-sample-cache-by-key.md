---
title: Azure API Management'ta özel önbelleğe alma
description: Azure API Yönetimi'nde öğeleri anahtarla nasıl önbelleğe alınacağını öğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 922ab731ccd76e6a1336d61abe4b0251e358beb7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60780830"
---
# <a name="custom-caching-in-azure-api-management"></a>Azure API Management'ta özel önbelleğe alma
Azure API Yönetimi hizmeti, kaynak URL'yi anahtar olarak kullanarak [HTTP yanıtı önbelleğe alma](api-management-howto-cache.md) için yerleşik destek ekibe sahiptir. Anahtar özellikleri kullanarak istek üstbilgi `vary-by` tarafından değiştirilebilir. Bu, tüm HTTP yanıtlarının (aka gösterimleri) önbelleğe almak için yararlıdır, ancak bazen yalnızca bir gösterimin bir kısmını önbelleğe almak yararlıdır. Yeni [önbellek arama değeri](/azure/api-management/api-management-caching-policies#GetFromCacheByKey) ve [önbellek deposu değeri](/azure/api-management/api-management-caching-policies#StoreToCacheByKey) ilkeleri, ilke tanımları içinden rasgele veri parçalarını depolama ve alma olanağı sağlar. Bu yetenek, artık dış hizmetlerden gelen yanıtları önbelleğe alabileceğinizden, daha önce tanıtılan [gönder isteği](/azure/api-management/api-management-advanced-policies#SendRequest) ilkesine de değer katar.

## <a name="architecture"></a>Mimari
API Yönetimi hizmeti, birden çok birime ölçeklendirdiğiniz gibi aynı önbelleğe alınmış verilere erişmeye devam edebilmeniz için paylaşılan kiracı başına paylaşılan bir veri önbelleği kullanır. Ancak, çok bölgeli bir dağıtımla çalışırken, her bölgede bağımsız önbellekler vardır. Önbelleği, bazı bilgilerin tek kaynağının olduğu bir veri deposu olarak ele alınmamak önemlidir. Bunu yaptıysanız ve daha sonra çok bölgeli dağıtımdan yararlanmaya karar verdiyseniz, seyahat eden kullanıcılara sahip müşteriler önbelleğe alınmış verilere erişimi kaybedebilir.

## <a name="fragment-caching"></a>Parça önbelleğe alma
Yanıtların döndürüldüğü bazı durumlar, belirlemek için pahalı olan verilerin bir kısmını içerir ve ancak makul bir süre için taze kalır. Örnek olarak, uçuş rezervasyonları, uçuş durumu, vb. ile ilgili bilgi sağlayan bir havayolu tarafından oluşturulmuş bir hizmeti göz önünde bulundurun. Kullanıcı havayolları puan programının bir üyesiyse, mevcut durumları ve birikmiş kilometre bilgilerine de sahip olacaktır. Kullanıcıyla ilgili bu bilgiler farklı bir sistemde depolanabilir, ancak uçuş durumu ve rezervasyonlar hakkında döndürülen yanıtlara eklemek isteolabilir. Bu, parça önbelleğe alma adı verilen bir işlem kullanılarak yapılabilir. Birincil gösterim, kullanıcıyla ilgili bilgilerin nereye ekleneceğini belirtmek için bir tür belirteç kullanarak kaynak sunucudan döndürülebilir. 

Arka uç API'sinden aşağıdaki JSON yanıtını düşünün.

```json
{
  "airline" : "Air Canada",
  "flightno" : "871",
  "status" : "ontime",
  "gate" : "B40",
  "terminal" : "2A",
  "userprofile" : "$userprofile$"
}  
```

Ve `/userprofile/{userid}` ikincil kaynak şöyle görünüyor:

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

Dahil edilmesi gereken uygun kullanıcı bilgilerini belirlemek için API Yönetimi'nin son kullanıcının kim olduğunu belirlemesi gerekir. Bu mekanizma uygulamaya bağlıdır. Örnek olarak, bir `Subject` `JWT` belirteç iddiasını kullanıyorum. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

API Yönetimi `enduserid` değeri daha sonra kullanmak üzere bir bağlam değişkeninde depolar. Bir sonraki adım, önceki bir isteğin kullanıcı bilgilerini zaten alıp alıp almadığını ve önbellekte depolayıp saklamadığını belirlemektir. Bunun için, API `cache-lookup-value` Yönetimi ilkeyi kullanır.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Önbellekte anahtar değerine karşılık gelen giriş yoksa, `userprofile` bağlam değişkeni oluşturulmaz. API `choose` Yönetimi, denetim akışı ilkesini kullanarak aramanın başarısını denetler.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

Bağlam `userprofile` değişkeni yoksa, API Yönetimi'nin onu almak için bir HTTP isteğinde bulunması gerekir.

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points to the profile for the current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

API Yönetimi, `enduserid` kullanıcı profili kaynağının URL'sini oluşturmak için kullanır. API Yönetimi yanıtı aldıktan sonra, gövde metnini yanıttan çıkarır ve bir bağlam değişkenine geri yerle bir eder.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

API Yönetimi'nin bu HTTP isteğini tekrar yapmasını önlemek için, aynı kullanıcı başka bir istekte bulununca, kullanıcı profilini önbellekte depolamayı belirtebilirsiniz.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

API Yönetimi, API Management'ın başlangıçta almaya çalıştığı anahtarı kullanarak önbellekteki değeri depolar. API Yönetimi'nin değeri depolamayı seçtiği süre, bilgilerin ne sıklıkta değiştiğine ve kullanıcıların güncel olmayan bilgilere ne kadar toleranslı olduğuna bağlı olmalıdır. 

Önbellekten geri alma işlemin hala bir iş dışı, ağ isteği ve potansiyel yine de isteğe onlarca milisaniye ekleyebilirsiniz olduğunu fark etmek önemlidir. Kullanıcı profili bilgilerinin belirlenmesinin yararları, veritabanı sorguları veya birden çok arka uçtan bilgi toplama gereksinimi nedeniyle bundan daha uzun sürer.

İşlemdeki son adım, döndürülen yanıtı kullanıcı profili bilgileriyle güncelleştirmektir.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Belirteçlerin bir parçası olarak tırnak işaretlerini eklemeyi seçebilirsiniz, böylece değiştirme gerçekleşmese bile yanıt yine de geçerli bir JSON olur.  

Tüm bu adımları bir araya getirdiğinizde, sonuç aşağıdaki gibi görünen bir ilkedir.

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in the cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If API Management doesn’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request to get user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points to the profile for the current end-user -->
                    <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>

                <!-- Store response body in context variable -->
                <set-variable
                  name="userprofile"
                  value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                <!-- Store result in cache -->
                <cache-store-value
                  key="@("userprofile-" + context.Variables["enduserid"])"
                  value="@((string)context.Variables["userprofile"])"
                  duration="100000" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <!-- Update response body with user profile-->
        <find-and-replace
              from='"$userprofile$"'
              to="@((string)context.Variables["userprofile"])" />
        <base />
    </outbound>
</policies>
```

Bu önbelleğe alma yaklaşımı, html'in sunucu tarafında oluşturulduğü web sitelerinde kullanılır, böylece tek bir sayfa olarak işlenebilir. Ayrıca, istemci tarafı HTTP önbelleğe alamaz veya istemci üzerinde bu sorumluluğu koymak için değil arzu edilir API'lerde yararlı olabilir.

Bu tür bir parça önbelleğe alma da bir Redis önbelleğe alma sunucusu kullanarak arka uç web sunucularında yapılabilir, ancak önbelleğe alınan parçalar birincil farklı arka uçlardan geliyorsa bu işi gerçekleştirmek için API Yönetimi hizmetini kullanarak yararlıdır Yanıt.

## <a name="transparent-versioning"></a>Saydam sürüm
Bir API'nin birden çok farklı uygulama sürümü için herhangi bir anda desteklenmesi yaygın bir uygulamadır. Örneğin, farklı ortamları (geliştirme, test, üretim, vb.) desteklemek veya API tüketicilerine daha yeni sürümlere geçiş için zaman vermek için API'nin eski sürümlerini desteklemek. 

Bunu işlemek için bir yaklaşım, istemci geliştiricilerin URL'leri `/v1/customers` `/v2/customers` değiştirmek yerine tüketicinin profil verilerinde şu anda kullanmak istedikleri API sürümünü depolamak ve uygun arka uç URL'sini aramaktır. Belirli bir istemci için aramak için doğru arka uç URL belirlemek için bazı yapılandırma verileri sorgulamak için gereklidir. Bu yapılandırma verilerini önbelleğe alarak, API Yönetimi bu aramayapmanın performans cezasını en aza indirebilir.

İlk adım, istenen sürümü yapılandırmak için kullanılan tanımlayıcıyı belirlemektir. Bu örnekte, sürümü ürün abonelik anahtarıyla ilişkilendirmeyi seçtim. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

API Yönetimi daha sonra zaten istenen istemci sürümünü aldı olup olmadığını görmek için bir önbellek arama yapar.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Ardından, API Yönetimi önbellekte bulup bulmadığını görmek için denetler.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

API Yönetimi bulamamışsa, API Yönetimi onu alır.

```xml
<send-request
    mode="new"
    response-variable-name="clientconfiguresponse"
    timeout="10"
    ignore-error="true">
            <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
            <set-method>GET</set-method>
</send-request>
```

Yanıt gövdesi metnini yanıttan ayıklayın.

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

İleride kullanmak üzere önbelleğe geri saklayın.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

Ve son olarak istemci tarafından istenen hizmetin sürümünü seçmek için arka uç URL'sini güncelleştirin.

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

Tam ilke aşağıdaki gibidir:

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If API Management doesn’t find it in the cache, make a request for it and store it -->
    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">
            <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
            </send-request>
            <!-- Store response body in context variable -->
            <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
            <!-- Store result in cache -->
            <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
        </when>
    </choose>
    <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
</inbound>
```

API tüketicilerinin istemcileri güncelleştirmek ve yeniden dağıtmak zorunda kalmadan istemciler tarafından hangi arka uç sürümüne erişildiğini şeffaf bir şekilde denetlemelerini sağlamak, birçok API sürüm kaygısını gideren zarif bir çözümdür.

## <a name="tenant-isolation"></a>Kiracı İzolasyon
Daha büyük, çok kiracılı dağıtımlarda bazı şirketler, arka uç donanımının farklı dağıtımlarında ayrı kiracı grupları oluşturur. Bu, arka uçtaki bir donanım sorunundan etkilenen müşteri sayısını en aza indirir. Ayrıca, yeni yazılım sürümlerinin aşamalı olarak kullanıma sunulmasını da sağlar. İdeal olarak bu arka uç mimarisi API tüketicilere şeffaf olmalıdır. Api anahtarı başına yapılandırma durumu kullanarak arka uç URL'yi manipüle etme tekniğini temel aldığı için, bu durum saydam sürümle benzer bir şekilde elde edilebilir.  

Her abonelik anahtarı için API'nin tercih edilen bir sürümünü döndürmek yerine, kiracıyı atanan donanım grubuna ilişkilendiren bir tanımlayıcı döndürebilirsiniz. Bu tanımlayıcı uygun arka uç URL'sini oluşturmak için kullanılabilir.

## <a name="summary"></a>Özet
Her türlü veriyi depolamak için Azure API yönetim önbelleğini kullanma özgürlüğü, gelen isteğin işlenme şeklini etkileyebilecek yapılandırma verilerine verimli erişim sağlar. Ayrıca, arka uç API'den döndürülen yanıtları artırabilen veri parçalarını depolamak için de kullanılabilir.
