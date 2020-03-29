---
title: Bing Yerel İş Arama API v7 Başvuru
titleSuffix: Azure Cognitive Services
description: Bu makalede, yanıt nesneleri ve sorgu parametreleri ve arama sonuçlarını etkileyen üstbilgiler hakkında teknik ayrıntılar sağlar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: db764a73aa1bb18ef2fc0f8f6e5ffe8fd60d388c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74075689"
---
# <a name="bing-local-business-search-api-v7-reference"></a>Bing Yerel İş Arama API v7 başvuru

Yerel İş Arama API'si, restoranlar, oteller veya diğer yerel işletmeleri içeren sonuçlar almak için Bing'e bir arama sorgusu gönderir. Yerler için sorgu, yerel işletmenin veya bir kategorinin (örneğin, yakınımdaki restoranların) adını belirtebilir. Varlık sonuçları kişileri, yerleri veya nesneleri kapsar. Bu bağlamda yer iş varlıkları, devletler, ülkeler / bölgeler, vb.  

Bu bölümde yanıt nesneleri ve sorgu parametreleri ve arama sonuçlarını etkileyen üstbilgiler hakkında teknik ayrıntılar sağlar. İstek nasıl yapılacağını gösteren örnekler için Bkz. [Yerel İş Arama C# quickstart](quickstarts/local-quickstart.md) veya [Yerel İş Arama Java quickstart.](quickstarts/local-search-java-quickstart.md) 
  
İsteklerin içermesi gereken üstbilgiler hakkında bilgi için [bkz.](#headers)  
  
İstekleri içermesi gereken sorgu parametreleri hakkında bilgi için [sorgu parametrelerine](#query-parameters)bakın.  
  
Yanıtın içerdiği JSON nesneleri hakkında bilgi için [yanıt nesnelerine](#response-objects)bakın.

İzin verilen kullanım ve sonuçların görüntülenmesi hakkında bilgi için [kullanım ve görüntüleme gereksinimlerine](use-display-requirements.md)bakın.


  
## <a name="endpoint"></a>Uç Nokta  
Yerel işletme sonuçları istemek için bir GET isteği gönderin: 

``` 
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search

```
  
İstek, HTTPS protokolünü kullanmalıdır.  
  
> [!NOTE]
> En yüksek URL uzunluğu 2.048 karakterdir. URL uzunluğunun sınırı aşmadığından emin olmak için sorgu parametrelerinizin maksimum uzunluğu 1.500 karakterden az olmalıdır. URL 2.048 karakteri aşarsa, sunucu bulunamadı 404 döndürür.  
  
  
## <a name="headers"></a>Üst bilgiler  
Aşağıda, bir istek ve yanıtın içerebileceği üstbilgi ve üstbilgi ve bunlar yer almaktadır.  
  
|Üst bilgi|Açıklama|  
|------------|-----------------|  
|Kabul Et|İsteğe bağlı istek üst bilgisi.<br /><br /> Varsayılan ortam türü uygulama/json'dur. Yanıt Kullanımı [JSON-LD](https://json-ld.org/)belirtmek için, uygulama / ld +json kabul üstbilgi ayarlayın.|  
|<a name="acceptlanguage" />Accept-Language|İsteğe bağlı istek üst bilgisi.<br /><br /> Kullanıcı arabirimi dizelerinde kullanılacak virgülle sınırlanmış bir dil listesi. Liste, tercih edilme durumuna göre azalan düzende sıralanır. Beklenen biçim de içinde olmak üzere daha fazla bilgi için bkz. [RFC2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Bu üst bilgi ve [](#setlang)setLang&mdash; sorgu parametresi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin.<br /><br /> Bu üst bilgiyi ayarlarsanız, cc sorgu parametresini de belirtmelisiniz. Hangi pazardan sonuç döndürüleceğini belirlemek için, Bing listeden bulduğu ilk desteklenen dili kullanır ve bunu `cc` parametresinin değeriyle birleştirir. Liste desteklenen bir dil içermiyorsa, Bing isteği destekleyen en yakın dili ve pazarı bulur ya da sonuçlar için toplu veya varsayılan bir pazar kullanır. Bing'in kullandığı pazarı saptamak için BingAPIs-Market üst bilgisine bakın.<br /><br /> Ancak birden çok dil belirtirseniz bu üst bilgiyi ve `cc` sorgu parametresini kullanın. Aksi takdirde, [mkt](#mkt) ile [setLang](#setlang) sorgu parametrelerini kullanın.<br /><br /> Kullanıcı arabirimi dizesi, kullanıcı arabiriminde etiket olarak kullanılan dizedir. JSON yanıt nesnelerinde çok az kullanıcı arabirimi dizesi vardır. Yanıt nesnelerinde Bing.com özelliklerine yönelik bağlantılar da belirtilen dildedir.|  
|<a name="market" />BingAPIs-Market|Yanıt üst bilgisi.<br /><br /> İstek tarafından kullanılan pazar. Biçimi şöyledir: \<languageCode\>-\<countryCode\>. Örneğin, tr-TR.|  
|<a name="traceid" />BingAPIs-TraceId|Yanıt üst bilgisi.<br /><br /> İsteğin ayrıntılarını içeren günlük girdisinin kimliği. Hata oluştuğunda, bu kimliği yakalayın. Sorunu belirleyemez ve çözemezseniz, Destek ekibine diğer bilgilerle birlikte bu kimliği de sağlayın.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Gerekli istek üst bilgisi.<br /><br /> [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/)'de bu hizmete kaydolduğunuzda aldığınız abonelik anahtarı.|  
|<a name="pragma" />Pragma|İsteğe bağlı istek üst bilgisi<br /><br /> Varsayılan olarak, Bing önbelleğe alınmış içeriği (varsa) döndürür. Bing'in önbelleğe alınmış içeriği döndürmesini önlemek için, Pragma üst bilgisini no-cache olarak ayarlayın (örneğin, Pragma: no-cache).
|<a name="useragent" />User-Agent|İsteğe bağlı istek üst bilgisi.<br /><br /> İsteği başlatan kullanıcı aracısı. Bing, mobil kullanıcılara iyileştirilmiş bir deneyim sağlamak için kullanıcı aracısını kullanır. İsteğe bağlı olsa da, bu üst bilgiyi her zaman belirtmeniz önerilir.<br /><br /> User-agent, yaygın olarak kullanılan tarayıcılardan gönderilen dizeyle aynı olmalıdır. Kullanıcı aracıları hakkında bilgi için bkz. [RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Aşağıda örnek user-agent dizelerini bulabilirsiniz.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; like Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) like Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU OS 7_0 like Mac OS X) AppleWebKit/537.51.1 (KHTML, like Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|İsteğe bağlı istek ve yanıt üst bilgisi.<br /><br /> Bing, kullanıcılara tüm Bing API çağrılarında tutarlı bir davranış sağlamak için bu üst bilgiyi kullanır. Bing sık sık yeni özellikler ve geliştirmeler dağıtır ve farklı dağıtımlarda trafik ataması yapmak için anahtar olarak istemci kimliğini kullanır. Bir kullanıcı için birden çok istekte aynı istemci kimliğini kullanmazsanız, Bing kullanıcıyı birden çok çakışan dağıtıma atayabilir. Birden çok çakışan dağıtıma eklenmek, tutarsız bir kullanıcı deneyimine yol açabilir. Örneğin, ikinci isteğin dağıtım ataması ilkinden farklıysa, beklenmeyen bir deneyim yaşanabilir. Ayrıca, Bing istemci kimliğini kullanarak web sonuçlarını istemci kimliğinin arama geçmişine uyarlayabilir ve bu sayede kullanıcıya daha zengin bir deneyim sağlayabilir.<br /><br /> Bing, istemci kimliği tarafından oluşturulan etkinliği analiz ederek sonuç derecelendirmelerini geliştirmeye yardımcı olması için de bu üst bilgiyi kullanabilir. İlgi geliştirmeleri Bing API'lerinin daha kaliteli sonuçlar vermesine yardımcı olur ve böylelikle API tüketicisi için daha yüksek tıklama oranları getirir.<br /><br /> **ÖNEMLİ:** İsteğe bağlı olsa da, bu üst bilgiyi gerekli olarak kabul edebilirsiniz. Aynı son kullanıcı ile cihaz bileşimi için birden çok istekte aynı istemci kimliğini kullanıldığında, 1) API tüketicisi tutarlı bir kullanıcı deneyimi elde eder ve 2) Bing API'lerinden daha kaliteli sonuçlar alındığından tıklama oranları daha yüksek olur.<br /><br /> Bu üst bilgi için geçerli olan temel kullanım kuralları şunlardır:<br /><ul><li>Cihazda uygulamanızı kullanan her kullanıcının Bing tarafından oluşturulan benzersiz bir istemci kimliği olmalıdır.<br /><br/>İsteğe bu üst bilgiyi eklemezseniz, Bing bir kimlik oluşturur ve bu kimliği X-MSEdge-ClientID yanıt üst bilgisinde döndürür. İsteğe bu üst bilgiyi EKLEMEMENİZ gereken tek durum, söz konusu cihazda kullanıcının uygulamanızı ilk kez kullanmasıdır.<br /><br/></li><li>Cihazda uygulamanızın bu kullanıcı için yaptığı her Bing API'si isteğinde istemci kimliğini kullanın.<br /><br/></li><li>**Dikkat:** Bu İstemci Kimliğinin herhangi bir gerçekleştirilebilir kullanıcı hesabı bilgisine bağlanamayan olduğundan emin olmalısınız.</li><br/><li>İstemci kimliğinin kalıcı olmasını sağlayın. Tarayıcı uygulamasında kimliği kalıcı hale getirmek için, tüm oturumlarda kimliğin kullanmasını sağlayacak bir kalıcı HTTP tanımlama bilgisi kullanın. Oturum tanımlama bilgisi kullanmayın. Mobil uygulamalar gibi diğer uygulamalarda, kimliği kalıcı hale getirmek için cihazın kalıcı depolamasını kullanın.<br /><br/>Kullanıcı o cihazda uygulamanızı yeniden kullandığında, kalıcı hale getirdiğiniz istemci kimliğini alın.</li></ul><br /> **NOT:** Bing yanıtları bu üst bilgiyi içerebilir veya içermeyebilir. Yanıt bu üst bilgiyi içeriyorsa, istemci kimliğini yakalayın ve o cihazda kullanıcı için bunu izleyen tüm Bing isteklerinde onu kullanın.<br /><br /> **NOT:** X-MSEdge-ClientID üst bilgisini eklerseniz, isteğe tanımlama bilgileri eklememelisiniz.|  
|<a name="clientip" />X-MSEdge-ClientIP|İsteğe bağlı istek üst bilgisi.<br /><br /> İstemci cihazının IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgisini kullanarak güvenli arama davranışını saptar.<br /><br /> **NOT:** İsteğe bağlı olsa da, bu üst bilgiyi ve X-Search-Location üst bilgisini her zaman belirtmeniz önerilir.<br /><br /> Adresi karartmayın (örneğin, son sekiz karakteri 0'la değiştirerek). Adresin karartılması, cihazın gerçek konumuna yakın olmayan bir konum sonucu verir ve bu da Bing'in hatalı sonuçlar sağlamasına yol açabilir.|  
|<a name="location" />X-Search-Location|İsteğe bağlı istek üst bilgisi.<br /><br /> İstemcinin coğrafi konumunu açıklayan noktalı virgülle sınırlanmış anahtar/değer çifti listesi. Bing konum bilgisini kullanarak güvenli arama davranışını saptar ve ilgili yerel içeriği döndürür. Anahtar/değer çiftini \<anahtar\>:\<değer\> olarak belirtin. Aşağıda, kullanıcının konumunu belirtmek için kullandığınız anahtarlar gösterilir.<br /><br /><ul><li>lat&mdash;Derece olarak, müşterinin konumunun enlem. Enlem -90,0 değerinden büyük veya bu değere eşit ve +90,0 değerinden küçük veya bu değere eşit olmalıdır. Negatif değerler güney enlemlerini ve pozitif değerler de kuzey enlemlerini gösterir.<br /><br /></li><li>uzun&mdash;derece olarak, müşterinin konumunun boylam. Boylam -180,0 değerinden büyük veya bu değere eşit ve +180,0 değerinden küçük veya bu değere eşit olmalıdır. Negatif değerler batı boylamlarını ve pozitif değerler de doğu boylamlarını gösterir.<br /><br /></li><li>re&mdash; Koordinatların yatay doğruluğunu belirten metre yarıçapı. Cihazın konum hizmeti tarafından döndürülen değeri geçirin. Normalde değerler GPS/Wi-Fi için 22 m, baz istasyonu triangülasyonu için 380 m ve ters IP araması için 18.000 m'dir.<br /><br /></li><li>&mdash; istemcinin bulunduğu yerin UTC UNIX zaman damgası. (UNIX zaman damgası 1 Ocak 1970'den başlayarak saniye sayısıdır.)<br /><br /></li><li>head&mdash;İsteğe bağlı. İstemcinin göreli seyahat yönü. Gerçek kuzeye göre saat yönünün tersine 0 ile 360 derece arasında bir seyahat yönü belirtin. Bu anahtarı ancak `sp` anahtarı sıfırdan farklı bir değer olduğunda belirtin.<br /><br /></li><li>sp&mdash; Yatay hız (hız), saniyede metre, istemci cihaz seyahat ediyor.<br /><br /></li><li>alt&mdash; Müşteri cihazının yüksekliği, metre.<br /><br /></li><li>are&mdash;İsteğe bağlı. Koordinatların dikey doğruluğunu belirten metre cinsinden yarıçap. Yarıçap varsayılan olarak 50 Kilometre'dir. Bu anahtarı ancak `alt` anahtarı belirttiğiniz durumda belirtin.<br /><br /></li></ul> **NOT:** Bu anahtarlar isteğe bağlı olsa da, ne kadar çok bilgi sağlarsanız, konum sonuçları o kadar doğrudur.<br /><br /> **NOT:** Kullanıcının coğrafi konumunu her zaman belirtmeniz tavsiye edilir. İstemcinin IP adresi kullanıcının fiziksel konumunu doğru yansıtmıyorsa (örneğin istemci VPN kullanıyorsa), konumun belirtilmesi özellikle önemlidir. En iyi sonuçları elde etmek için, bu üst bilgiyi ve X-MSEdge-ClientIP üst bilgisini eklemelisiniz; ama en azından bu üst bilgiyi eklemeniz gerekir.|

> [!NOTE] 
> Kullanım Koşulları'nın, bu üst bilgilerin kullanımıyla ilgili olanlar da dahil olmak üzere tüm ilgili yasalara uymayı gerektirdiğini unutmayın. Örneğin, Avrupa gibi bazı yasama bölgelerinde kullanıcı cihazlarına izleme cihazları takmadan önce kullanıcının iznini almak gerekir.
  

## <a name="query-parameters"></a>Sorgu parametreleri  
İstek aşağıdaki sorgu parametrelerini içerebilir. Gerekli parametreler için Gerekli sütuna bakın. Sorgu parametrelerini URL olarak kodlamanız gerekir.  
  
  
|Adı|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|
|<a name="count" />count|`offset` Parametre tarafından belirtilen dizin ile başlayan, döndürülecek sonuç sayısı.|Dize|Hayır|   
|<a name="localCategories" />localKategoriler|İş kategorisine göre aramayı tanımlayan seçenekler listesi.  [Bkz. Yerel İşletme Kategorileri arama](local-categories.md)|Dize|Hayır|  
|<a name="mkt" />mkt|Sonuçların geldiği pazar. <br /><br />Olası piyasa değerlerinin listesi için, Piyasa Kodları'na bakın.<br /><br /> **NOT:** Yerel İş Arama API'si şu anda yalnızca en-us pazarını ve dilini desteklemektedir.<br /><br />|Dize|Evet|
|<a name="offset"/>Uzaklık|`count` Parametre tarafından belirtilen sonuçları başlatmak için dizin.|Tamsayı|Hayır|  
|<a name="query" />S|Kullanıcının arama terimi.|Dize|Hayır|  
|<a name="responseformat" />responseFormat|Yanıt için kullanılacak ortam türü. Olası büyük/küçük harf duyarsız değerleri aşağıda veda edilebilmektedir.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> Varsayılan json olduğunu. Yanıtın içerdiği JSON nesneleri hakkında bilgi için [Yanıt Nesneleri'ne](#response-objects)bakın.<br /><br />  JsonLd belirtirseniz, yanıt gövdesi arama sonuçlarını içeren JSON-LD nesnelerini içerir. JSON-LD hakkında bilgi için, [JSON-LD](https://json-ld.org/)bakın.|Dize|Hayır|  
|<a name="safesearch" />safeSearch|Yetişkinlere yönelik içeriği filtrelemek için kullanılan bir filtre. Aşağıdakiler, büyük/küçük harfe duyarlı olmayan olası filtre değerleridir.<br /><ul><li>Yetişkinlere uygun metin, resim veya videoiçeren Web sayfalarını Geri Getir'i kapatın.&mdash;<br /><br/></li><li>Yetişkinlere uygun metiniçeren Orta&mdash;Getir web sayfaları, ancak yetişkinlere uygun resimler veya videolar değil.<br /><br/></li><li>Katı&mdash;Web sayfalarını yetişkinlere uygun metin, resim veya videoyla döndürmeyin.</li></ul><br /> Varsayılan ayar Moderate değeridir.<br /><br /> **NOT:** İstek, Bing'in yetişkin politikasının `safeSearch` Gerektirdiği ve Katı olarak ayarlanan bir `safeSearch` pazardan geliyorsa, Bing değeri yok sayar ve Katı'yı kullanır.<br/><br/>**NOT:**`site:` sorgu işlecini kullanmanız durumunda, `safeSearch` parametresinin ayarına bakılmaksızın yanıtta yetişkinlere yönelik içerik bulunabilir. `site:` işlecini yalnızca sitenin içeriği hakkında bilgi sahibiyseniz ve senaryonuz, yetişkinlere yönelik içeriğin mevcut olma ihtimalini destekliyorsa kullanın. |Dize|Hayır|  
|<a name="setlang" />setLang|Kullanıcı arabirimi dizelerinde kullanılacak dil. Dili belirtirken ISO 639-1 2 harfi dil kodunu kullanın. Örneğin, Türkçe için dil kodu TR'dir. Varsayılan değer EN (İngilizce) ayarıdır.<br /><br /> İsteğe bağlı olsa da, her zaman dil belirtmelisiniz. Kullanıcı tarafından kullanıcı arabirimi dizelerinin farklı dilde görüntülenmesi istenmediği sürece, normalde `setLang` parametresini `mkt` parametresiyle aynı dile ayarlarsınız.<br /><br /> Bu parametre ve [](#acceptlanguage)Accept-Language&mdash; üst bilgisi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin.<br /><br /> Kullanıcı arabirimi dizesi, kullanıcı arabiriminde etiket olarak kullanılan dizedir. JSON yanıt nesnelerinde çok az kullanıcı arabirimi dizesi vardır. Ayrıca, yanıt nesnelerinde Bing.com özelliklerine yönelik bağlantılar da belirtilen dildedir.|Dize|Hayır| 


## <a name="response-objects"></a>Yanıt Nesneleri  
Aşağıda, yanıtın içerebileceği JSON yanıt nesneleri verebilirsiniz. İstek başarılı olursa, yanıttaki üst düzey nesne [SearchResponse](#searchresponse) nesnesidir. İstek başarısız olursa, üst düzey nesne [ErrorResponse](#errorresponse) nesnesidir.


|Nesne|Açıklama|  
|------------|-----------------|  
|[Yer](#place)|Restoran veya otel gibi yerel işletme hakkındaki bilgileri tanımlar.|  

  
### <a name="error"></a>Hata  
Oluşan hatayı tanımlar.  
  
|Öğe|Açıklama|Tür|  
|-------------|-----------------|----------|  
|<a name="error-code" />Kod|Hata kategorisini tanımlayan hata kodu. Olası kodların listesi için [Hata Kodları'na](#error-codes)bakın.|Dize|  
|<a name="error-message" />İleti|Hatanın açıklaması.|Dize|  
|<a name="error-moredetails" />moreAyrıntılar|Hata hakkında ek bilgi sağlayan bir açıklama.|Dize|  
|<a name="error-parameter" />Parametre|Hataya neden olan istekteki sorgu parametresi.|Dize|  
|<a name="error-subcode" />Subcode|Hatayı tanımlayan hata kodu. Örneğin, Geçersiz `code` İstek ise, `subCode` ParameterGeçersiz veya ParameterGeçersiz Değer olabilir. |Dize|  
|<a name="error-value" />value|Sorgu parametresi değeri geçerli değildi.|Dize|  
  

### <a name="errorresponse"></a>Hata Yanıtı  
İstek başarısız olduğunda yanıtın içerdiği üst düzey nesne.  
  
|Adı|Değer|Tür|  
|----------|-----------|----------|  
|_type|İpucu yazın.|Dize|  
|<a name="errors" />Hata|İsteğin neden başarısız olduğunu açıklayan hatalar listesi.|[Hata](#error)[]|  

  
  
### <a name="license"></a>Lisans  
Metnin veya fotoğrafın kullanılabileceği lisansı tanımlar.  
  
|Adı|Değer|Tür|  
|----------|-----------|----------|  
|ad|Lisansın adı.|Dize|  
|url|Kullanıcının lisans hakkında daha fazla bilgi alabilecekleri bir web sitesinin URL'si.<br /><br /> Köprü oluşturmak için adı ve URL'yi kullanın.|Dize|  


### <a name="link"></a>Bağlantı  
Köprü bileşenlerini tanımlar.  
  
|Adı|Değer|Tür|  
|----------|-----------|----------|  
|_type|İpucu yazın.|Dize|  
|metin|Görüntü metni.|Dize|  
|url|Bir URL. Köprü oluşturmak için URL'yi ve metni görüntüleyin.|Dize|  
  


  
### <a name="organization"></a>Kuruluş  
Bir yayımcı tanımlar.  
  
Bir yayımcının adını veya web sitesini veya her ikisini birden sağlayabileceğini unutmayın.  
  
|Adı|Değer|Tür|  
|----------|-----------|----------|  
|ad|Yayıncının adı.|Dize|  
|url|Yayımcının web sitesinin URL'si.<br /><br /> Yayımcının bir web sitesi sağlayamadığını unutmayın.|Dize|  
  
  

### <a name="place"></a>Yer  
Restoran veya otel gibi yerel bir işletme hakkındaki bilgileri tanımlar.  
  
|Adı|Değer|Tür|  
|----------|-----------|----------|  
|_type|Aşağıdakilerden birine ayarlanabilecek ipucu yazın:<br /><br /><ul><li>Otel</li><li>Yerel İşletme<br /></li><li>Restoran</ul><li>|Dize|  
|adres|Varlığın bulunduğu yerin posta adresi.|Posta Adresi|  
|entityPresentationInfo|Varlığın türünü belirlemek için kullanabileceğiniz ipuçları gibi varlık hakkında ek bilgiler. Örneğin, ister bir restoran ister otel olsun. Alan `entityScenario` ListItem olarak ayarlanır.|EntityPresentationInfo|  
|ad|Varlığın adı.|Dize|  
|Telefon|Varlığın telefon numarası.|Dize|  
|url|Varlığın web sitesinin URL'si.<br /><br /> Tıklandığında kullanıcıyı varlığın web sitesine götüren bir köprü oluşturmak için varlığın adı ile birlikte bu URL'yi kullanın.|Dize|  
|webSearchUrl|Bing'in bu yer için arama sonucunun URL'si.|Dize| 
  
  
### <a name="querycontext"></a>Querycontext  
Bing'in istek için kullandığı sorgu bağlamını tanımlar.  
  
|Öğe|Açıklama|Tür|  
|-------------|-----------------|----------|  
|adultIntent|Belirtilen sorgunun yetişkinlere uygun olup olmadığını gösteren bir Boolean değeri. Sorgunun yetişkin niyeti varsa değer **doğrudur;** aksi takdirde, **yanlış**.|Boole|  
|değişiklikOverrideQuery|Bing'i özgün dizeyi kullanmaya zorlamak için kullanılacak sorgu dizesi. Örneğin, sorgu dizesi *rüzgarı sallasa,* geçersiz kılma sorgu dizesi *+saling downwind*olacaktır. *%2Bsaling+rüzgarla*sonuçlanan sorgu dizesini kodlamayı unutmayın.<br /><br /> Bu alan yalnızca özgün sorgu dizesi yazım hatası içeriyorsa dahil edilir.|Dize|  
|alteredQuery|Bing tarafından sorguyu gerçekleştirmek için kullanılan sorgu dizesi. Bing, özgün sorgu dizesi yazım hataları içeriyorsa değiştirilmiş sorgu dizesini kullanır. Örneğin, sorgu dizesi `saling downwind`ise, değiştirilen sorgu `sailing downwind`dizesi .<br /><br /> Bu alan yalnızca özgün sorgu dizesi yazım hatası içeriyorsa dahil edilir.|Dize|  
|askUserForLocation|Bing'in doğru sonuçlar sağlamak için kullanıcının konumunu gerektirip gerektirmediğini gösteren bir Boolean değeri. [X-MSEdge-ClientIP](#clientip) ve [X-Search-Location](#location) üstbilgilerini kullanarak kullanıcının konumunu belirttiyseniz, bu alanı yoksayabilirsiniz.<br /><br /> Doğru sonuçlar sağlamak için kullanıcının konumuna ihtiyaç duyan "bugünün hava durumu" veya "yakınımdaki restoranlar" gibi konum farkında sorgular için bu alan **doğru**olarak ayarlanır.<br /><br /> Konumu içeren konum farkında sorgular için (örneğin, "Seattle hava durumu"), bu alan **yanlış**olarak ayarlanır. Bu alan, "en çok satanlar" gibi konum farkında olmayan sorgular için de **false** olarak ayarlanır.|Boole|  
|originalQuery|İstekte belirtildiği gibi sorgu dizesi.|Dize|  

### <a name="identifiable"></a>Tanımlana -bilen

|Adı|Değer|Tür|  
|-------------|-----------------|----------|
|id|Kaynak tanımlayıcısı|Dize|
 
### <a name="rankinggroup"></a>Sıralama Grubu
Ana hat gibi bir arama sonuçları grubunu tanımlar.

|Adı|Değer|Tür|  
|-------------|-----------------|----------|
|Bileşen|Grupta görüntülenecek arama sonuçları listesi.|Sıralama Öğesi|

### <a name="rankingitem"></a>Sıralama Öğesi
Görüntülenecek bir arama sonucu öğesi tanımlar.

|Adı|Değer|Tür|  
|-------------|-----------------|----------|
|sonuçIndex|Görüntülenecek yanıttaki öğenin sıfır tabanlı dizini. Öğe bu alanı içermiyorsa, yanıttaki tüm öğeleri görüntüleyin. Örneğin, Tüm haber makalelerini Haberler yanıtında görüntüleyin.|Tamsayı|
|answerType|Görüntülenecek öğeyi içeren yanıt. Örneğin, Haberler.<br /><br />Aramayı Yanıt nesnesinde yanıtı bulmak için türü kullanın. Tür, SearchResponse alanının adıdır.<br /><br /> Ancak, yanıt türünü yalnızca bu nesne değer alanını içeriyorsa kullanın; aksi takdirde, görmezden.|Dize|
|textualIndex|Görüntülemek için metinsel Yanıtlar cevap dizini.| İmzasız İntesiyon|
|value|Görüntülenecek bir yanıtı veya görüntülenecek bir yanıt öğesini tanımlayan kimlik. Kimlik bir yanıt tanımlıyorsa, yanıtın tüm öğelerini görüntüleyin.|Tanımlana -bilen|

### <a name="rankingresponse"></a>Sıralama Yanıtı  
Arama sonuçları sayfası içeriğinin nereye ve hangi sırada yerleştirileceğini tanımlar.  
  
|Adı|Değer|  
|----------|-----------|  
|<a name="ranking-mainline" />Mainline|Arama sonuçları ana satırda görüntülenecek.|  
|<a name="ranking-pole" />Kutup|En görünür tedavinin karşılanabilecek arama sonuçları (örneğin, ana çizgi ve kenar çubuğunun üzerinde görüntülenir).|  
|<a name="ranking-sidebar" />Kenar çubuğu|Kenar çubuğunda görüntülenecek arama sonuçları.| 

### <a name="searchresponse"></a>Searchresponse  
İstek başarılı olduğunda yanıtın içerdiği üst düzey nesneyi tanımlar.  
  
Hizmet, hizmet reddi saldırısından şüphelenirse, isteğin başarılı olacağını unutmayın (HTTP durum kodu 200 Ok'dur); ancak, yanıtın gövdesi boş olacaktır.  
  
|Adı|Değer|Tür|  
|----------|-----------|----------|  
|_type|SearchResponse olarak ayarlanmış ipucu yazın.|Dize|  
|Yerler|Arama sorgusuyla alakalı varlıkların listesi.|JSON nesnesi|  
|Querycontext|Bing'in istek için kullandığı sorgu dizesini içeren bir nesne.<br /><br /> Bu nesne, kullanıcı tarafından girilen sorgu dizesini içerir. Sorgu dizesi bir yazım hatası içeriyorsa, Bing'in sorgu için kullandığı değiştirilmiş bir sorgu dizesi de içerebilir.|[Querycontext](#querycontext)|  


## <a name="error-codes"></a>Hata kodları

Aşağıda, bir isteğin döndürdettiği olası HTTP durum kodları vereme olasılığı ve  
  
|Durum Kodu|Açıklama|  
|-----------------|-----------------|  
|200|Başarılı.|  
|400|Sorgu parametrelerinden biri eksik veya geçersiz.|  
|401|Abonelik anahtarı eksik veya geçerli değil.|  
|403|Kullanıcının kimliği doğrulanır (örneğin, geçerli bir abonelik anahtarı kullanmıştır) ancak istenen kaynağa izinleri yoktur.<br /><br /> Arayan kişinin sorgularını aylık kotasını aşması durumunda Bing de bu durumu döndürebilir.|  
|410|İstek, HTTPS protokolü yerine HTTP'yi kullandı. HTTPS desteklenen tek protokoldür.|  
|429|Arayan, sorgularını ikinci kota başına aştı.|  
|500|Beklenmeyen sunucu hatası.|

İstek başarısız olursa, yanıt hatanın neden olduğu açıklayan hata [nesnelerinin](#error) listesini içeren bir [Hata Yanıtı](#errorresponse) nesnesi içerir. Hata bir parametreyle ilgiliyse, `parameter` alan sorun olan parametreyi tanımlar. Hata bir parametre değeriyle ilişkiliyse, `value` alan geçerli olmayan değeri tanımlar.

```json
{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidRequest", 
      "subCode": "ParameterMissing", 
      "message": "Required parameter is missing.", 
      "parameter": "q" 
    }
  ]
}

{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidAuthorization", 
      "subCode": "AuthorizationMissing", 
      "message": "Authorization is required.", 
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

Olası hata kodu ve alt hata kodu değerleri aşağıda veda edilebilmektedir.

|Kod|Subcode|Açıklama
|-|-|-
|SunucuHatası|Beklenmeyen Hata<br/>Kaynak Hatası<br/>Uygulanmadı|HTTP durum kodu 500'dür.
|Geçersizİstek|ParametreEksik<br/>ParametreGeçersiz Değer<br/>httpİzin verilen<br/>Engellendi|Bing, isteğin herhangi bir bölümü geçerli olmadığında Geçersiz İstek'i döndürür. Örneğin, gerekli bir parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterGeçersizDeğer ise, HTTP durum kodu 400'dür.<br/><br/>HTTPS yerine HTTP protokolünü kullanırsanız, Bing HttpNotAllowed'i döndürür ve HTTP durum kodu 410'dur.
|RateLimitAşıldı|Alt kod yok|Bing, sorgularınızı saniyede (QPS) veya aylık sorgularınızı (QPM) kotanızı aştığınızda RateLimitExceeded'i döndürür.<br/><br/>QPS'yi aşarsanız, Bing HTTP durum kodu 429'u döndürür ve QPM'yi aşarsanız Bing 403'ü döndürür.
|Geçersiz Yetkilendirme|Yetkilendirme Eksik<br/>YetkilendirmeRedundancy|Bing, arayan kişinin kimliğini doğrulayamıyorsa Geçersiz Yetkilendirme'yi döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtirseniz artıklık oluşur.<br/><br/>Hata Geçersiz Yetkilendirme ise, HTTP durum kodu 401'dir.
|Yetersiz Yetkilendirme|YetkilendirmeDevre Dışı<br/>Yetkilendirme Süresi Doldu|Arayan kaynağa erişmek için izinleri yoksa Bing Yetersiz Yetkilendirme döndürür. Bu, abonelik anahtarı devre dışı bırakılmışsa veya süresi dolmuşsa oluşabilir. <br/><br/>Hata Yetersiz Yetkilendirme ise, HTTP durum kodu 403'dür.

## <a name="next-steps"></a>Sonraki adımlar
- [Yerel İş Arama sıcağınızda](quickstarts/local-quickstart.md)
- [Yerel İş Arama Java quickstart](quickstarts/local-search-java-quickstart.md)
- [Yerel İş Arama Düğümü hızlı başlat](quickstarts/local-search-node-quickstart.md)
- [Yerel İş Arama Python quickstart](quickstarts/local-search-python-quickstart.md)
