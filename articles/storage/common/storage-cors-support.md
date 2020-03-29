---
title: Çapraz Kaynak Paylaşımı (CORS) Desteği | Microsoft Dokümanlar
description: Microsoft Azure Depolama Hizmetleri için CORS Desteğini nasıl etkinleştireceklerini öğrenin.
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 2/22/2017
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: bb296db0d97382deac984369704777de5d5cb362
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65147682"
---
# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Azure Depolama Hizmetleri için Çapraz Kaynak Paylaşımı (CORS) Desteği
Azure depolama hizmetleri, 2013-08-15 sürümünden başlayarak Blob, Tablo, Kuyruk ve Dosya hizmetleri için Çapraz Kaynak Paylaşımını (CORS) destekler. CORS, bir etki alanı altında çalışan bir web uygulamasının başka bir etki alanındaki kaynaklara erişmesini sağlayan bir HTTP özelliğidir. Web tarayıcıları, bir web sayfasının farklı bir etki alanında API'leri aramasını engelleyen [aynı kaynak ilkesi](https://www.w3.org/Security/wiki/Same_Origin_Policy) olarak bilinen bir güvenlik kısıtlaması uygular; CORS, bir etki alanının (başlangıç etki alanının) başka bir etki alanında API'leri aramasına izin vermek için güvenli bir yol sağlar. CORS hakkında ayrıntılı bilgi için [CORS belirtimine](https://www.w3.org/TR/cors/) bakın.

Blob [Hizmet Özelliklerini Ayarla,](https://msdn.microsoft.com/library/hh452235.aspx) [Sıra Sıra Hizmet Özelliklerini Ayarla](https://msdn.microsoft.com/library/hh452232.aspx)ve Tablo [Hizmeti Özelliklerini Ayarla'yı](https://msdn.microsoft.com/library/hh452240.aspx)arayarak, depolama hizmetlerinin her biri için CORS kurallarını ayrı ayrı ayarlayabilirsiniz. Hizmet için CORS kurallarını ayarladıktan sonra, farklı bir etki alanından hizmete karşı yapılan doğru bir yetkilendirme isteği, belirttiğiniz kurallara göre izin verilip verilmediğini belirlemek için değerlendirilir.

> [!NOTE]
> CORS'in bir kimlik doğrulama mekanizması olmadığını unutmayın. CORS etkinleştirildiğinde depolama kaynağına karşı yapılan herhangi bir istek, uygun bir kimlik doğrulama imzasına sahip olmalıdır veya bir kamu kaynağına karşı yapılmalıdır.
> 
> 

## <a name="understanding-cors-requests"></a>CORS isteklerini anlama
Bir kaynak etki alanından gelen CORS isteği iki ayrı istekten oluşabilir:

* Hizmet tarafından uygulanan CORS kısıtlamalarını sorgulayan bir ön uçuş isteği. İstek yöntemi basit bir [yöntem](https://www.w3.org/TR/cors/)olmadığı sürece ön kontrol isteği gereklidir , get, HEAD veya POST anlamına gelir.
* İstenilen kaynağa karşı yapılan gerçek istek.

### <a name="preflight-request"></a>Ön kontrol isteği
Ön kontrol isteği, hesap sahibi tarafından depolama hizmeti için oluşturulan CORS kısıtlamalarını sorgular. Web tarayıcısı (veya diğer kullanıcı aracısı), istek üstbilgilerini, yöntemi ve kaynak etki alanını içeren bir OPTIONS isteği gönderir. Depolama hizmeti, bir depolama kaynağına karşı gerçek bir istekte hangi kaynak etki alanlarının, istek yöntemlerinin ve istek üstaylıklarının belirtilebileceğini belirten önceden yapılandırılmış BIR CORS kuralı kümesine dayanarak amaçlanan işlemi değerlendirir.

Hizmet için CORS etkinleştirilirse ve ön uçuş isteğiyle eşleşen bir CORS kuralı varsa, hizmet durum kodu 200 (Tamam) ile yanıt verir ve yanıtta gerekli Access-Control üstbilgilerini içerir.

Hizmet için CORS etkinleştirilmezse veya CORS kuralı ön uçuş isteğiyle eşleşmiyorsa, hizmet durum kodu 403 (Yasak) ile yanıt verecektir.

OPTIONS isteği gerekli CORS üstbilgileri (Kaynak ve Erişim Denetimi-İstek-Yöntem üstbilgileri) içermiyorsa, hizmet durum kodu 400 (Kötü istek) ile yanıt verir.

Ön kontrol isteğinin istenen kaynağa (Blob, Sıra ve Tablo) göre değerlendirildiğini unutmayın. Hesap sahibinin, isteğin başarılı olabilmesi için hesap hizmeti özelliklerinin bir parçası olarak CORS'u etkinleştirmiş olması gerekir.

### <a name="actual-request"></a>Gerçek istek
Ön uçuş isteği kabul edildikten ve yanıt döndürüldünden sonra, tarayıcı gerçek isteği depolama kaynağına karşı gönderir. Ön kontrol isteği reddedilirse tarayıcı gerçek isteği hemen reddeder.

Gerçek istek depolama hizmetine karşı normal istek olarak kabul edilir. Origin üstbilgisinin varlığı, isteğin BIR CORS isteği olduğunu ve hizmetin eşleşen CORS kurallarını denetleeceğini gösterir. Bir eşleşme bulunursa, Access-Control üstbilgisi yanıta eklenir ve istemciye geri gönderilir. Eşleşme bulunmazsa, CORS Access-Control üstbilgileri döndürülmez.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Azure Depolama hizmetleri için CORS'u etkinleştirme
CORS kuralları hizmet düzeyinde ayarlanır, bu nedenle her hizmet için CORS'u (Blob, Sıra ve Tablo) ayrı ayrı etkinleştirmeniz veya devre dışı bırakmalısınız. Varsayılan olarak CORS her hizmette devre dışı bırakılmıştır. CORS'i etkinleştirmek için, 2013-08-15 veya sonraki sürümlerini kullanarak uygun hizmet özelliklerini ayarlamanız ve hizmet özelliklerine CORS kuralları eklemeniz gerekir. Bir hizmet için CORS'in nasıl etkinleştirilen veya devre dışı bırakılabildiği ve CORS kurallarının nasıl ayarlanabileceği hakkında ayrıntılı bilgi için lütfen [Blob Hizmet Özelliklerini Ayarla,](https://msdn.microsoft.com/library/hh452235.aspx) [Sıra Hizmeti Özelliklerini Ayarla](https://msdn.microsoft.com/library/hh452232.aspx)ve Tablo [Hizmeti Özelliklerini Ayarla'ya](https://msdn.microsoft.com/library/hh452240.aspx)bakın.

Burada, Set Hizmeti Özellikleri işlemi yle belirtilen tek bir CORS kuralının bir örneği verilmiştir:

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

CORS kuralına dahil edilen her öğe aşağıda açıklanmıştır:

* **AllowedOrigins**: CORS aracılığıyla depolama hizmetine karşı istekte bulunmasına izin verilen başlangıç etki alanlarıdır. Kaynak etki alanı, isteğin kaynağı olan etki alanıdır. Kaynağın, kullanıcı yaşının hizmete gönderdiği kaynakla tam olarak büyük/küçük harf duyarlı bir eşleşme olması gerektiğini unutmayın. Ayrıca, tüm kaynak etki alanlarının CORS aracılığıyla istekte bulunabilmesi için joker karakter '*' de kullanabilirsiniz. Yukarıdaki örnekte, http: /www.contoso.com ve http:\/\//www.fabrikam.com, CORS kullanarak hizmete karşı istekte bulunabilir.
* **İzin Verilen Yöntemler**: Orijin etki alanının CORS isteği için kullanabileceği yöntemler (HTTP istek fiilleri). Yukarıdaki örnekte yalnızca PUT ve GET isteklerine izin verilir.
* **İzin Veren Kaydediciler**: Orijin etki alanının CORS isteğinde belirtebileceği istek üstbilgisi. Yukarıdaki örnekte, x-ms-meta-veri, x-ms-meta-hedef ve x-ms-meta-abc ile başlayan tüm meta veri başlıklarına izin verilir. Joker karakter '*' belirtilen önek ile başlayan herhangi bir üstbilgi izin verildiğini gösterir unutmayın.
* **ExposedHeaders**: CORS isteğine yanıt olarak gönderilebilecek ve tarayıcı tarafından istek verene açıkta kalan yanıt başlıkları. Yukarıdaki örnekte, tarayıcı x-ms-meta ile başlayan herhangi bir üstbilgi ortaya çıkarmak için talimat verilir.
* **MaxAgeInSeconds**: Bir tarayıcının ön kontrol SEÇENEKLERI isteğini önbelleğe alması gereken maksimum tutar süresi.

Azure depolama hizmetleri, hem **İzin Kaydediciler** hem de **ExposedHeaders** öğeleri için önceden belirlenmiş üstbilgileri belirtmeyi destekler. Üstbilgi kategorisine izin vermek için, bu kategoriye ortak bir önek belirtebilirsiniz. Örneğin, önceden belirlenmiş bir üstbilgi olarak *x-ms-meta** belirterek x-ms-meta ile başlayan tüm üstbilgi eşleşecek bir kural kurar.

Aşağıdaki sınırlamalar CORS kuralları için geçerlidir:

* Depolama hizmeti başına en fazla beş CORS kuralı (Blob, Tablo ve Sıra) belirtebilirsiniz.
* XML etiketleri hariç, istekteki tüm CORS kuralları ayarlarının maksimum boyutu 2 KB'yi geçmemelidir.
* İzin verilen üstbilginin, açıkta kalan üstbilginin veya izin verilen kaynağın uzunluğu 256 karakteri geçmemelidir.
* İzin verilen üstbilgi ve açıkta kalan üstbilgi aşağıdakiler olabilir:
  * Tam üstbilgi adının sağlandığı, **x-ms-meta-işlenmiş**gibi gerçek üstbilgi. İstekte en fazla 64 gerçek üstbilgi belirtilebilir.
  * **x-ms-meta-data*** gibi üstbilginin önekinin sağlandığı önceden belirlenmiş üstbilgiler. Bu şekilde bir önek belirtilmesi, verilen önek ile başlayan herhangi bir üstbilgi sağlar veya ortaya çıkarır. İstekte en fazla iki önceden sabitlenmiş üstbilgi belirtilebilir.
* **İzin Verilen Yöntemler** öğesinde belirtilen yöntemler (veya HTTP fiilleri), Azure depolama hizmeti API'leri tarafından desteklenen yöntemlere uygun olmalıdır. Desteklenen yöntemler DELETE, GET, HEAD, MERGE, POST, OPTIONS ve PUT'tır.

## <a name="understanding-cors-rule-evaluation-logic"></a>CORS kural değerlendirme mantığını anlamak
Bir depolama hizmeti ön kontrol veya gerçek bir istek aldığında, bu isteği uygun Servis Özellikleri Kümesi işlemi aracılığıyla hizmet için oluşturduğunuz CORS kurallarına göre değerlendirir. CORS kuralları, Set Hizmet Özellikleri işleminin istek gövdesinde ayarlandığı sırada değerlendirilir.

CORS kuralları aşağıdaki gibi değerlendirilir:

1. İlk olarak, isteğin başlangıç etki **alanı, AllowedOrigins** öğesi için listelenen etki alanlarıyla karşılaştırılır. Kaynak etki alanı listeye dahil edilirse veya tüm etki alanlarıjoker karakter '*' ile izin verilirse, kural değerlendirmesi devam eder. Kaynak etki alanı dahil edilmezse, istek başarısız olur.
2. Daha sonra, isteğin yöntemi (veya HTTP fiili) **İzin Verilen Yöntemler** öğesinde listelenen yöntemlerle karşılaştırılır. Yöntem listeye dahil edilirse, kural değerlendirmesi devam eder; değilse, istek başarısız olur.
3. İstek, kaynak etki alanında ve yöntemindeki bir kuralla eşleşirse, isteği işlemek için bu kural seçilir ve başka kural değerlendirilmez. Ancak, istek başarılı olmadan önce, istekte belirtilen **üstbilgi, İzin Kaydediciler** öğesinde listelenen üstbilgiyle karşılaştırılır. Gönderilen üstbilgi izin verilen üstbilgiyle eşleşmiyorsa, istek başarısız olur.

Kurallar istek gövdesinde mevcut oldukları sırayla işlendiğinden, en iyi uygulamalar, önce listede kökenlerle ilgili en kısıtlayıcı kuralları belirtmenizi önerir, böylece bunlar önce değerlendirilir. Listenin sonunda, tüm kökenlere izin veren bir kural olan daha az kısıtlayıcı kurallar belirtin.

### <a name="example--cors-rules-evaluation"></a>Örnek – CORS kuralları değerlendirme
Aşağıdaki örnek, depolama hizmetleri için CORS kurallarını ayarlamak için bir işlem için kısmi bir istek gövdesini gösterir. [Bkz. Blob Hizmet Özelliklerini Ayarla,](https://msdn.microsoft.com/library/hh452235.aspx) [Sıra Hizmet Özelliklerini Ayarla](https://msdn.microsoft.com/library/hh452232.aspx)ve istek oluşturmayla ilgili ayrıntılar için Tablo Hizmeti Özelliklerini [Ayarla.](https://msdn.microsoft.com/library/hh452240.aspx)

```xml
<Cors>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>PUT,HEAD</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>*</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
    </CorsRule>
</Cors>
```

Ardından, aşağıdaki CORS isteklerini göz önünde bulundurun:

| İstek |  |  | Yanıt |  |
| --- | --- | --- | --- | --- |
| **Yöntem** |**Kaynak** |**İstek Üst Bilgileri** |**Kural Maç** |**Sonuç** |
| **PUT** |http:\//www.contoso.com |x-ms-blob-içerik türü |İlk kural |Başarılı |
| **Al** |http:\//www.contoso.com |x-ms-blob-içerik türü |İkinci kural |Başarılı |
| **Al** |http:\//www.contoso.com |x-ms-istemci-istek-id |İkinci kural |Hata |

İlk istek ilk kuralla eşleşir – kaynak etki alanı izin verilen kökenlerle eşleşir, yöntem izin verilen yöntemlerle eşleşir ve üstbilgi izin verilen üstbilgiyle eşleşir ve böylece başarılı olurlar.

Yöntem izin verilen yöntemlerle eşleşmediği için ikinci istek ilk kuralla eşleşmiyor. Ancak, ikinci kuralmaç, böylece başarılı olur.

Üçüncü istek, kaynak etki alanı ve yöntemindeki ikinci kuralla eşleşir, bu nedenle başka kural değerlendirilmez. Ancak, *x-ms-istemci-istek-id üstbilgi* ikinci kural tarafından izin verilmez, bu nedenle istek başarısız, üçüncü kuralın semantik başarılı izin olurdu rağmen.

> [!NOTE]
> Bu örnek, daha kısıtlayıcı bir kural önce daha az kısıtlayıcı bir kural gösterse de, genel olarak en iyi yöntem önce en kısıtlayıcı kuralları listelemektir.
> 
> 

## <a name="understanding-how-the-vary-header-is-set"></a>Vary üstbilgisinin nasıl ayarlanır olduğunu anlama
*Vary* üstbilgi, tarayıcıya veya kullanıcı aracısına isteği işlemek için sunucu tarafından seçilen ölçütler hakkında bilgi veren bir dizi istek üstbilgi alanından oluşan standart bir HTTP/1.1 üstbilgidir. *Vary* üstbilgisi, yanıtın nasıl önbelleğe alınması gerektiğini belirlemek için kullanan yakınlıklar, tarayıcılar ve CDN'ler tarafından önbelleğe almak için kullanılır. Ayrıntılar için [Vary üstbilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)nin belirtimine bakın.

Tarayıcı veya başka bir kullanıcı aracısı cors isteğinden yanıtı önbelleğe aldığında, kaynak etki alanı izin verilen kaynak olarak önbelleğe alınır. Önbellek etkinken ikinci bir etki alanı depolama kaynağı için aynı isteği verdiğinde, kullanıcı aracısı önbelleğe alınan kaynak etki alanını alır. İkinci etki alanı önbelleğe alınmış etki alanıyla eşleşmez, bu nedenle istek aksi takdirde başarılı olduğunda başarısız olur. Bazı durumlarda, Azure Depolama, kullanıcı aracısına, istenen etki alanı önbelleğe alınan kaynaktan farklı olduğunda sonraki CORS isteğini hizmete göndermesi için talimat vermek üzere **Başlangıç** için Vary üstbilgisini ayarlar.

Azure Depolama, aşağıdaki durumlarda gerçek GET/HEAD istekleri için **Başlangıç** için *Vary* üstbilgisini ayarlar:

* İstek kaynağı, CORS kuralıyla tanımlanan izin verilen menşele tam olarak eşleştiğinde. Tam eşleşme olması için, CORS kuralı joker karakter ' * ' karakterini içermeyebilir.
* İstek kaynağıyla eşleşen bir kural yoktur, ancak depolama hizmeti için CORS etkinleştirilir.

GET/HEAD isteğinin tüm kökenlere izin veren bir CORS kuralıyla eşleşmesi durumunda, yanıt tüm kökenlere izin verildiğini gösterir ve kullanıcı aracısı önbelleği önbellek etkinken herhangi bir kaynak etki alanından sonraki isteklere izin verir.

GET/HEAD dışındaki yöntemleri kullanan isteklerde, bu yöntemlere verilen yanıtlar kullanıcı aracıları tarafından önbelleğe alınmadığından, depolama hizmetleri Vary üstbilgisini ayarlamaz.

Aşağıdaki tablo, Azure depolama nın daha önce belirtilen durumlara göre GET/HEAD isteklerine nasıl yanıt vereceğini gösterir:

| İstek | Hesap ayarı ve kural değerlendirmesinin sonucu |  |  | Yanıt |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| **İstek üzerine başlangıç üstbilgisi mevcut** |**Bu hizmet için belirtilen CORS kural(lar)** |**Tüm kökenlere izin veren eşleşen kural vardır(*)** |**Tam kaynak eşleşmesi için eşleştirme kuralı vardır** |**Yanıt, Başlangıç olarak ayarlanmış değişken üstbilgi içerir** |**Yanıt, Access-Control-Allowed-Origin içerir: "*"** |**Yanıt, Access-Control-Exposed-Headers içerir** |
| Hayır |Hayır |Hayır |Hayır |Hayır |Hayır |Hayır |
| Hayır |Evet |Hayır |Hayır |Evet |Hayır |Hayır |
| Hayır |Evet |Evet |Hayır |Hayır |Evet |Evet |
| Evet |Hayır |Hayır |Hayır |Hayır |Hayır |Hayır |
| Evet |Evet |Hayır |Evet |Evet |Hayır |Evet |
| Evet |Evet |Hayır |Hayır |Evet |Hayır |Hayır |
| Evet |Evet |Evet |Hayır |Hayır |Evet |Evet |

## <a name="billing-for-cors-requests"></a>CORS istekleri için faturalandırma
Hesabınızdaki depolama hizmetlerinden herhangi biri için CORS'i etkinleştirdiyseniz [(Blob Hizmet Özelliklerini Ayarla,](https://msdn.microsoft.com/library/hh452235.aspx) [Sıra Hizmet Özelliklerini Ayarla](https://msdn.microsoft.com/library/hh452232.aspx)veya Tablo [Hizmet Özelliklerini Ayarla)](https://msdn.microsoft.com/library/hh452240.aspx)arayarak başarılı ön uçuş istekleri faturalandırılır. Ücretleri en aza indirmek için, CORS kurallarınızdaki **MaxAgeInSeconds** öğesini, kullanıcı aracısının isteği önbelleğe alabilmesi için büyük bir değere ayarlamayı düşünün.

Başarısız ön kontrol istekleri faturalandırılmayacak.

## <a name="next-steps"></a>Sonraki adımlar
[Blob Servis Özelliklerini Ayarla](https://msdn.microsoft.com/library/hh452235.aspx)

[Sıra Hizmet Özelliklerini Ayarla](https://msdn.microsoft.com/library/hh452232.aspx)

[Tablo Hizmet Özelliklerini Ayarla](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C Çapraz Kaynak Paylaşımı Belirtimi](https://www.w3.org/TR/cors/)

