---
title: Azure API Management'ta özel önbelleğe alma
description: Azure API Management öğelerini anahtara göre önbelleğe alma hakkında bilgi edinin
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "60780830"
---
# <a name="custom-caching-in-azure-api-management"></a>Azure API Management'ta özel önbelleğe alma
Azure API Management hizmetinde, anahtar olarak kaynak URL 'SI kullanılarak [http yanıtı önbelleğe alma](api-management-howto-cache.md) için yerleşik destek bulunur. Anahtar, özellikler kullanılarak istek üstbilgileri tarafından değiştirilebilir `vary-by` . Bu, tüm HTTP yanıtlarının (diğer adıyla gösterimler) önbelleğe alınması için yararlıdır, ancak bazen bir gösterimin bir bölümünü önbelleğe almak için yararlıdır. Yeni [önbellek-arama-değer](/azure/api-management/api-management-caching-policies#GetFromCacheByKey) ve [önbellek-depolama-değer](/azure/api-management/api-management-caching-policies#StoreToCacheByKey) ilkeleri, ilke tanımlarından rastgele veri parçalarını depolama ve alma olanağı sağlar. Artık dış hizmetlerden gelen yanıtları önbelleğe sunabileceğinden, bu özellik önceden tanıtılan [gönderme isteği](/azure/api-management/api-management-advanced-policies#SendRequest) ilkesine değer de ekler.

## <a name="architecture"></a>Mimari
API Management hizmet, birden çok birime kadar ölçeklendirerek aynı önbelleğe alınmış verilere erişmeye devam ettiğiniz için paylaşılan bir kiracı veri önbelleği kullanır. Ancak, çok bölgeli bir dağıtımla çalışırken her bir bölgenin içinde bağımsız önbellekler vardır. Önbellek bir veri deposu olarak değerlendirilmemelidir, burada bazı bilgilerin tek kaynağı olduğu yerdir. Bunu yaptıysanız ve daha sonra çok bölgeli dağıtımdan faydalanmaya karar verdiyseniz, kullanıcılara seyahat eden kullanıcılar bu önbelleğe alınmış verilere erişimi kaybedebilir.

## <a name="fragment-caching"></a>Parça önbelleğe alma
Verilerin döndürülmekte olduğu bazı durumlar, belirli bir süre içinde belirlenmesi ve henüz yeni olmaya devam etmek pahalı olan verilerin bir kısmını içerir. Örnek olarak, uçak rezervasyonları, uçuş durumu vb. ile ilgili bilgi sağlayan bir hava yolu tarafından oluşturulan bir hizmeti düşünün. Kullanıcı Airlines Points programının bir üyesiyse, geçerli durumu ve birikmiş harcırah ile ilgili bilgiler de bulunur. Kullanıcıyla ilgili bu bilgiler farklı bir sistemde depolanabilir, ancak bunu uçuş durumu ve ayırmaları hakkında döndürülen yanıtlara eklemek istenebilir. Bu, parça önbelleğe alma adlı bir işlem kullanılarak yapılabilir. Birincil temsili, Kullanıcı ile ilgili bilgilerin nereye ekleneceğini göstermek üzere bir tür belirteç kullanılarak kaynak sunucudan döndürülebilir. 

Bir arka uç API 'sinden aşağıdaki JSON yanıtını göz önünde bulundurun.

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

Ve `/userprofile/{userid}` şunun gibi görünen ikincil kaynak

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

Dahil edilecek uygun Kullanıcı bilgilerini belirlemek için, son kullanıcının kim olduğunu belirlemek API Management gerekir. Bu mekanizma uygulamaya bağımlıdır. Örnek olarak, `Subject` bir belirteç talebini kullanıyorum `JWT` . 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

API Management, `enduserid` daha sonra kullanmak üzere değeri bir bağlam değişkeninde depolar. Sonraki adım, önceki bir isteğin kullanıcı bilgilerini zaten almıştır ve önbellekte depolanmadığını belirlemektir. Bu, API Management `cache-lookup-value` ilkeyi kullanır.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Önbellekte anahtar değerine karşılık gelen hiçbir giriş yoksa, hiçbir `userprofile` bağlam değişkeni oluşturulmaz. API Management, denetim akışı ilkesini kullanarak aramanın başarısını denetler `choose` .

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

`userprofile`Bağlam değişkeni yoksa API Management almak için BIR http isteği olması gerekir.

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

API Management, `enduserid` Kullanıcı profili kaynağına URL 'yi oluşturmak için öğesini kullanır. API Management yanıt aldıktan sonra, gövde metnini yanıtın dışına çeker ve bir bağlam değişkenine geri depolar.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

API Management bu HTTP isteğini yeniden yapmasını önlemek için, aynı kullanıcı başka bir istek yaptığında kullanıcı profilini önbellekte depolamayı belirtebilirsiniz.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

API Management, API Management ilk olarak onu almaya çalıştığı anahtarın aynısını kullanarak önbellekteki değeri depolar. Değeri depolamak için API Management seçtiği süre, bilgilerin ne sıklıkta değiştiği ve dayanıklı kullanıcıların güncel olmayan bilgilere göre olması gerekir. 

Önbellekten alma işleminin hala bir işlem dışı olduğunu, ağ isteğini ve potansiyel olarak isteğe hiç sayıda milisaniye ekleyebileceğini unutmamak önemlidir. Avantajlar, Kullanıcı profili bilgilerinin veritabanı sorguları yapması veya birden çok arka uçta bilgi toplaması nedeniyle daha uzun sürmesini belirlemede gelir.

İşlemdeki son adım, döndürülen yanıtı Kullanıcı profili bilgileriyle güncelleştirmedir.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Kilit işaretlerini belirtecin bir parçası olarak eklemeyi tercih edebilirsiniz; böylece, Değiştir gerçekleşmese bile yanıt hala geçerli bir JSON olur.  

Tüm bu adımları birlikte birleştirdiğinizde, son sonuç aşağıdakine benzer bir ilkedir.

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

Bu önbelleğe alma yaklaşımı öncelikle HTML 'nin sunucu tarafında oluşturulduğu web sitelerinde kullanılır, böylece tek bir sayfa olarak işlenebilirler. İstemcilerin istemci tarafı HTTP önbelleği yapamayan API 'lerde de yararlı olabilir veya bu sorumluluğu istemciye yerleştirmemelidir.

Bu aynı tür parça önbelleğe alma özelliği, arka uç Web sunucularında redin Caching Server kullanılarak da yapılabilir, ancak, önbelleğe alınan parçaların birincil yanıtlardan farklı arka uçlardan geldiği durumlarda bu çalışmayı gerçekleştirmek için API Management hizmetini kullanmak faydalıdır.

## <a name="transparent-versioning"></a>Saydam sürüm oluşturma
Bir API 'nin birden çok farklı uygulama sürümünün tek seferde desteklenmesi yaygın bir uygulamadır. Örneğin, farklı ortamları (geliştirme, test, üretim, vb.) desteklemek veya API tüketicilerinin daha yeni sürümlere geçirilmesi için zaman kazandırmak üzere API 'nin eski sürümlerini desteklemek için. 

İstemci geliştiricilerinin URL 'Leri ' dan ' a değiştirmesini istemek yerine, bu işlemleri işlemek için bir yaklaşım, bu `/v1/customers` `/v2/customers` API 'nin şu anda kullanmak istedikleri API sürümünü ve uygun arka uç URL 'sini çağırabilmesi için, istemci geliştiricilerin ' den ' den ' a kadar olan Belirli bir istemci için çağrılacak doğru arka uç URL 'sini belirlemekte, bazı yapılandırma verilerinin sorgulanmasý gerekir. Bu yapılandırma verilerini önbelleğe alarak API Management, bu aramanın performans cezası en aza indirebilir.

İlk adım, istenen sürümü yapılandırmak için kullanılan tanımlayıcıyı belirlemektir. Bu örnekte, sürümü ürün abonelik anahtarıyla ilişkilendirmeyi seçtim. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

API Management daha sonra, istenen istemci sürümünü zaten alınmış olup olmadığını görmek için bir önbellek araması yapar.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Ardından, API Management önbellekte bulamamış olup olmadığını denetler.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

API Management bulamazsa API Management alır.

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

Daha sonra kullanmak üzere önbelleğe geri depolayın.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

Son olarak, arka uç URL 'sini istemci tarafından istenen hizmetin sürümünü seçmek üzere güncelleştirin.

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

Tüm ilke aşağıdaki gibidir:

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

API tüketicilerini, istemcileri güncelleştirme ve yeniden dağıtmaya gerek kalmadan istemciler tarafından hangi arka uç sürümünün eriştiğini saydam olarak denetlemesine olanak tanımak, birçok API sürümü oluşturma sorunlarını ele alan zarif bir çözümdür.

## <a name="tenant-isolation"></a>Kiracı yalıtımı
Daha büyük, çok kiracılı dağıtımlar bazı şirketler, arka uç donanımının farklı dağıtımlarında ayrı kiracı grupları oluşturur. Bu, arka uçta bir donanım sorunundan etkilenen müşterilerin sayısını en aza indirir. Ayrıca, yeni yazılım sürümlerinin aşamalar halinde kullanıma alınmasına de olanak sağlar. İdeal olarak, bu arka uç mimarisinin API tüketicilerine şeffaf olması gerekir. Bu,, API anahtarı başına yapılandırma durumu kullanılarak arka uç URL 'sini düzenleme tekniğinin temel aldığı için, saydam sürüm için benzer bir şekilde sağlanabilir.  

Her bir abonelik anahtarı için API 'nin tercih edilen bir sürümünü döndürmek yerine, bir kiracıyı atanmış donanım grubuyla ilişkilendiren bir tanımlayıcı döndürürsınız. Bu tanımlayıcı, uygun arka uç URL 'sini oluşturmak için kullanılabilir.

## <a name="summary"></a>Özet
Her türlü veri depolamak için Azure API Management önbelleğini kullanma özgürlüğü, bir gelen isteğin işlenme yöntemini etkileyebilecek yapılandırma verilerine verimli bir şekilde erişim sağlar. Ayrıca, bir arka uç API 'sinden döndürülen yanıtları geliştiren veri parçalarını depolamak için de kullanılabilir.
