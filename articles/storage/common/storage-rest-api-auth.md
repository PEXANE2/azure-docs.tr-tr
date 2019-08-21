---
title: Paylaşılan anahtar yetkilendirmesi ile Azure depolama hizmetleri REST API işlemlerini çağırma | Microsoft Docs
description: Paylaşılan anahtar yetkilendirmesi kullanarak blob depolamaya bir istek yapmak için Azure depolama REST API kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 1463a470c84d38ebc30e32cf539aa9d6f64a6854
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640674"
---
# <a name="using-the-azure-storage-rest-api"></a>Azure Depolama REST API’sini kullanma

Bu makalede, blob Storage hizmeti REST API 'Lerinin nasıl kullanılacağı ve hizmete yapılan çağrıya nasıl yetki vereceğiniz gösterilmektedir. REST hakkında hiçbir şey bilen bir geliştiricinin görünüm noktasından yazılmıştır ve bir REST çağrısının nasıl yapılacağını fikir vermez. Bir REST çağrısının başvuru belgelerine baktık ve bunu gerçek bir REST çağrısına nasıl çevitireceğiz: hangi alanlar nereye gidebileceği? REST çağrısını ayarlamayı öğrendikten sonra, diğer depolama hizmeti REST API 'Lerinden birini kullanmak için bu bilgilerden yararlanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar 

Uygulama, depolama hesabı için blob depolamadaki kapsayıcıları listeler. Bu makaledeki kodu denemek için aşağıdaki öğeler gereklidir: 

* Aşağıdaki iş yüküyle [Visual Studio 2019](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) ' i yükledikten sonra:
    - Azure geliştirme

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Genel amaçlı depolama hesabı. Henüz bir depolama hesabınız yoksa, bkz. [depolama hesabı oluşturma](storage-quickstart-create-account.md).

* Bu makaledeki örnekte, bir depolama hesabındaki kapsayıcıların nasıl listeyapılacağı gösterilmektedir. Çıktıyı görmek için, başlamadan önce depolama hesabındaki blob depolamaya bazı kapsayıcılar ekleyin.

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Örnek uygulama, içinde C#yazılmış bir konsol uygulamasıdır.

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın. 

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Visual Studio çözümünü açmak için, Storage-DotNet-REST-API-with-auth klasörünü arayın, açın ve StorageRestApiAuth. sln ' ye çift tıklayın. 

## <a name="what-is-rest"></a>REST nedir?

REST, *temsili durum aktarımı*anlamına gelir. Belirli bir tanım için, [Vikipedi](https://en.wikipedia.org/wiki/Representational_state_transfer)'e göz atın.

Temel olarak, REST API 'Leri çağırırken veya API 'Leri çağrılabilir hale getirerek kullanabileceğiniz bir mimaridir. Bu, her iki tarafta da neler olduğunu ve REST çağrıları gönderirken veya alırken kullanılan diğer yazılımları bağımsız olarak kullanır. Mac, Windows, Linux, Android telefon veya tablet, iPhone, iPod veya Web sitesinde çalışan bir uygulama yazabilir ve bu platformların hepsi için aynı REST API kullanabilirsiniz. REST API çağrıldığında veriler geçirilebilir ve/veya out olabilir. REST API, hangi platformın çağırdığına göre değil, istek içinde geçirilen bilgiler ve yanıtta verilen veriler ne önemlidir.

REST 'in nasıl kullanılacağını bilmek yararlı bir beceriye sahiptir. Azure ürün ekibi sıklıkla yeni özellikler yayınlar. Birçok kez, yeni özelliklere REST arabiriminden erişilebilir. Bazen, Özellikler **Tüm** depolama istemci kitaplıklarının veya kullanıcı arabiriminden (Azure Portal gibi) ilerlenmemiş olabilir. Her zaman en son ve en iyi sürümünü kullanmak istiyorsanız öğrenme REST bir gereksinimdir. Ayrıca, Azure depolama ile etkileşim kurmak için kendi kitaplığınızı yazmak istiyorsanız veya bir SDK ya da depolama istemci kitaplığı olmayan bir programlama diliyle Azure depolamaya erişmek istiyorsanız REST API kullanabilirsiniz.

## <a name="about-the-sample-application"></a>Örnek uygulama hakkında

Örnek uygulama, bir depolama hesabındaki kapsayıcıları listeler. REST API belgelerindeki bilgilerin gerçek kodunuzla nasıl ilişkili olduğunu anladıktan sonra diğer REST çağrılarına daha kolay bir şekilde ulaşın. 

[BLOB hizmeti REST API](/rest/api/storageservices/Blob-Service-REST-API)bakarsanız, blob depolamada gerçekleştirebileceğiniz tüm işlemleri görürsünüz. Depolama istemci kitaplıkları REST API 'Leri etrafında sarmalayıcılardır; REST API 'Lerini doğrudan kullanmadan depolamaya erişiminizi kolaylaştırır. Ancak yukarıda belirtildiği gibi, bazen depolama istemci kitaplığı yerine REST API kullanmak isteyebilirsiniz.

## <a name="rest-api-reference-list-containers-api"></a>REST API başvurusu: Liste kapsayıcıları API 'SI

[Listcontainers](/rest/api/storageservices/List-Containers2) işlemi Için REST API başvurusuyla sayfaya bakalım. Bu bilgiler, bazı alanların istek ve yanıtta ne yerde olduğunu anlamanıza yardımcı olur.

**Istek yöntemi**: AL. Bu fiil, istek nesnesinin bir özelliği olarak belirttiğiniz HTTP yöntemidir. Bu fiil için diğer değerler, aradığınız API 'ye bağlı olarak HEAD, PUT ve DELETE ' i içerir.

**İstek URI 'si**: https://myaccount.blob.core.windows.net/?comp=list Bu, BLOB depolama hesabı uç `http://myaccount.blob.core.windows.net` noktasından ve kaynak dizeden `/?comp=list` oluşturulur.

[URI parametreleri](/rest/api/storageservices/List-Containers2#uri-parameters): ListContainers çağrılırken kullanabileceğiniz ek sorgu parametreleri vardır. Bu parametrelerin birkaç ikisi, filtreleme için kullanılan çağrı (saniye cinsinden) ve *ön ek*için *zaman* aşımındır.

Diğer bir faydalı parametre *MaxResults:* bu değerden daha fazla kapsayıcı varsa, yanıt gövdesi bir sonraki istekte döndürülecek bir sonraki kapsayıcıyı gösteren bir *nextişaretleyici* öğesi içerir. Bu özelliği kullanmak için, bir sonraki isteği yaptığınızda URI 'de *işaret* parametresi olarak *nextişaretleyici* değerini sağlarsınız. Bu özelliği kullanırken, sonuçlar üzerinde sayfalama yapmak benzerdir. 

Ek parametreleri kullanmak için, bu örneğe benzer şekilde, değeri kaynak dizesine ekleyin:

```
/?comp=list&timeout=60&maxresults=100
```

[Istek üst bilgileri](/rest/api/storageservices/List-Containers2#request-headers) **:** Bu bölümde, gerekli ve isteğe bağlı istek üstbilgileri listelenmektedir. Üst bilgilerden üçü gereklidir: bir *Yetkilendirme* üst bilgisi, *x-MS-Date* (isteğin UTC saatini içerir) ve *x-ms-Version* (kullanılacak REST API sürümünü belirtir). Üst bilgilerde *x-MS-Client-Request-ID* dahil, isteğe bağlıdır: Bu alanın değerini herhangi bir şekilde ayarlayabilirsiniz; günlüğe kaydetme etkinleştirildiğinde depolama Analizi günlüklerine yazılır.

[Istek gövdesi](/rest/api/storageservices/List-Containers2#request-body) **:** ListContainers için bir istek gövdesi yok. İstek gövdesi, Bloblar karşıya yüklenirken tüm PUT işlemlerinde kullanılır ve bu da SetContainerAccessPolicy bir depolanan erişim ilkeleri XML listesinde göndermenizi sağlar. Saklı erişim ilkeleri, [paylaşılan erişim imzaları (SAS) kullanılarak](storage-sas-overview.md)makalesinde ele alınmıştır.

[Yanıt durum kodu](/rest/api/storageservices/List-Containers2#status-code) **:** Bilmeniz gereken herhangi bir durum koduna söyler. Bu örnekte, 200 HTTP durum kodu Tamam ' dır. HTTP durum kodlarının tüm listesi için, [durum kodu tanımlarını](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)inceleyin. Depolama REST API 'Lerine özgü hata kodlarını görmek için bkz. [ortak REST API hata kodları](/rest/api/storageservices/common-rest-api-error-codes)

[Yanıt üst bilgileri](/rest/api/storageservices/List-Containers2#response-headers) **:** Bunlar *Içerik türünü*içerir; *x-MS-Request-ID*, GEÇIRILEN istek kimliği; kullanılan blob hizmeti sürümünü gösteren *x-MS-Version*; UTC 'de olan ve isteğin ne zaman yapıldığını bildiren *Tarih*.

[Yanıt gövdesi](/rest/api/storageservices/List-Containers2#response-body): Bu alan, istenen verileri sağlayan bir XML yapısıdır. Bu örnekte, yanıt kapsayıcıların ve bunların özelliklerinin bir listesidir.

## <a name="creating-the-rest-request"></a>REST isteği oluşturuluyor

Başlatmadan önce birkaç Not: üretimde çalışırken güvenlik için her zaman HTTP yerine HTTPS kullanın. Bu alıştırmanın amaçları doğrultusunda, istek ve yanıt verilerini görüntüleyebilmeniz için HTTP kullanmanız gerekir. Gerçek REST çağrılarında istek ve yanıt bilgilerini görüntülemek için [Fiddler](https://www.telerik.com/fiddler) 'ı veya benzer bir uygulamayı indirebilirsiniz. Visual Studio çözümünde, depolama hesabı adı ve anahtarı sınıfında sabit olarak kodlanmıştır. ListContainersAsyncREST yöntemi, depolama hesabı adını ve depolama hesabı anahtarını REST isteğinin çeşitli bileşenlerini oluşturmak için kullanılan yöntemlere geçirir. Gerçek bir dünya uygulamasında, depolama hesabı adı ve anahtarı bir yapılandırma dosyasında, ortam değişkenlerinde veya bir Azure Key Vault alınacaksa bulunur.

Örnek projemizdeki yetkilendirme üstbilgisini oluşturma kodu ayrı bir sınıfta bulunur. Fikir, tüm sınıfı alıp kendi çözümünüze ekleyebilir ve "olduğu gibi" kullanabilirsiniz. Yetkilendirme üst bilgisi kodu, Azure depolama 'ya birçok REST API çağrısı için geçerlidir.

Bir HttpRequestMessage nesnesi olan isteği oluşturmak için Program.cs içindeki ListContainersAsyncREST sayfasına gidin. İstek oluşturma adımları şunlardır: 

* Hizmeti çağırmak için kullanılacak URI 'yi oluşturun. 
* HttpRequestMessage nesnesini oluşturun ve yükü ayarlayın. ' De bir şeyi geçirdiğimiz için yük ListContainersAsyncREST için null.
* X-MS-Date ve x-MS-Version için istek üst bilgilerini ekleyin.
* Yetkilendirme üst bilgisini alın ve ekleyin.

İhtiyaç duyduğunuz bazı temel bilgiler: 

*  ListContainers için yöntemi. `GET` Bu değer, istek örneği oluşturulurken ayarlanır. 
*  **Kaynak** , hangi API 'nin ÇAĞRıLDıĞıNı belirten URI 'nin sorgu bölümüdür, yani değer `/?comp=list`. Daha önce belirtildiği gibi, kaynak, [Listcontainers API 'si](/rest/api/storageservices/List-Containers2)hakkındaki bilgileri gösteren başvuru belgeleri sayfasında bulunur.
*  URI, bu depolama hesabı için blob hizmeti uç noktası oluşturularak ve kaynağı birleştirerek oluşturulur. **İstek URI 'si** değeri sona erer `http://contosorest.blob.core.windows.net/?comp=list`.
*  ListContainers için **Requestbody** null ve ek **üst bilgi**yok.

Farklı API 'Ler, *IfMatch*gibi geçirilecek başka parametrelere sahip olabilir. PutBlob 'U çağırırken ifMatch 'i kullanmanın bir örneği. Bu durumda, ifMatch öğesini eTag ile ayarlarsınız ve yalnızca sağladığınız eTag, blobdaki geçerli eTag ile eşleşiyorsa blob 'u günceller. ETag 'i almasından bu yana başka biri blobu güncelleştirdiyse, yaptıkları değişiklikler geçersiz kılınmayacaktır. 

İlk olarak, `uri` `payload`ve öğesini ayarlayın. 

```csharp
// Construct the URI. This will look like this:
//   https://myaccount.blob.core.windows.net/resource
String uri = string.Format("http://{0}.blob.core.windows.net?comp=list", storageAccountName);

// Set this to whatever payload you desire. Ours is null because 
//   we're not passing anything in.
Byte[] requestPayload = null;
```

Sonra, isteği örnek oluşturun ve URI 'yi sağlamak için `GET` metodunu olarak ayarlar.

```csharp 
//Instantiate the request message with a null payload.
using (var httpRequestMessage = new HttpRequestMessage(HttpMethod.Get, uri)
{ Content = (requestPayload == null) ? null : new ByteArrayContent(requestPayload) })
{
```

X-MS-Date ve x-MS-Version için istek üst bilgilerini ekleyin. Kodda bu yer, çağrı için gereken ek istek üstbilgilerini de eklediğiniz yerdir. Bu örnekte, ek üst bilgi yok. Ek üstbilgilere geçen bir API örneği SetContainerACL ' dir. BLOB depolama için, "x-MS-blob-genel-erişim" adlı bir üst bilgi ve erişim düzeyi için değer ekler.

```csharp
    // Add the request headers for x-ms-date and x-ms-version.
    DateTime now = DateTime.UtcNow;
    httpRequestMessage.Headers.Add("x-ms-date", now.ToString("R", CultureInfo.InvariantCulture));
    httpRequestMessage.Headers.Add("x-ms-version", "2017-07-29");
    // If you need any additional headers, add them here before creating
    //   the authorization header. 
```

Yetkilendirme üstbilgisini oluşturan ve istek üst bilgilerine ekleyen yöntemi çağırın. Makalede daha sonra yetkilendirme üst bilgisini nasıl oluşturacağınız hakkında bilgi edineceksiniz. Yöntem adı, bu kod parçacığında görebileceğiniz GetAuthorizationHeader ' dır:

```csharp
    // Get the authorization header and add it.
    httpRequestMessage.Headers.Authorization = AzureStorageAuthenticationHelper.GetAuthorizationHeader(
        storageAccountName, storageAccountKey, now, httpRequestMessage);
```

Bu noktada, `httpRequestMessage` yetkilendirme üst bilgileriyle Rest isteği tamamlanma bilgilerini içerir. 

## <a name="call-the-rest-api-with-the-request"></a>REST API isteğiyle çağırın

Artık isteğiniz olduğuna göre, REST isteğini göndermek için Sendadsync öğesini çağırabilirsiniz. Sendadsync, API 'YI çağırır ve yanıtı geri alır. Yanıt StatusCode ' i inceleyin (200 Tamam), ardından yanıtı ayrıştırın. Bu durumda, kapsayıcıların XML bir listesini alırsınız. İsteği oluşturmak için GetRESTRequest yöntemini çağırma koduna bakalım, isteği yürütün ve sonra kapsayıcı listesi için yanıtı inceleyin.

```csharp 
    // Send the request.
    using (HttpResponseMessage httpResponseMessage = 
      await new HttpClient().SendAsync(httpRequestMessage, cancellationToken))
    {
        // If successful (status code = 200), 
        //   parse the XML response for the container names.
        if (httpResponseMessage.StatusCode == HttpStatusCode.OK)
        {
            String xmlString = await httpResponseMessage.Content.ReadAsStringAsync();
            XElement x = XElement.Parse(xmlString);
            foreach (XElement container in x.Element("Containers").Elements("Container"))
            {
                Console.WriteLine("Container name = {0}", container.Element("Name").Value);
            }
        }
    }
}
```

Senpasync çağrısı yaparken [Fiddler](https://www.telerik.com/fiddler) gibi bir ağ algılayıcısı çalıştırırsanız, istek ve yanıt bilgilerini görebilirsiniz. Şimdi bir göz atalım. Depolama hesabının adı *contosorest*' dir.

**İsteyen**

```
GET /?comp=list HTTP/1.1
```

**İstek üst bilgileri:**

```
x-ms-date: Thu, 16 Nov 2017 23:34:04 GMT
x-ms-version: 2014-02-14
Authorization: SharedKey contosorest:1dVlYJWWJAOSHTCPGiwdX1rOS8B4fenYP/VrU0LfzQk=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Yürütme sonrasında döndürülen durum kodu ve yanıt üstbilgileri:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 3e889876-001e-0039-6a3a-5f4396000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 00:23:42 GMT
Content-Length: 1511
```

**Yanıt gövdesi (XML):** ListContainers için bu, kapsayıcıların ve bunların özelliklerinin listesini gösterir.

```xml  
<?xml version="1.0" encoding="utf-8"?>
<EnumerationResults 
  ServiceEndpoint="http://contosorest.blob.core.windows.net/">
  <Containers>
    <Container>
      <Name>container-1</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:39:48 GMT</Last-Modified>
        <Etag>"0x8D46CBD5A7C301D"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-2</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:40:50 GMT</Last-Modified>
        <Etag>"0x8D46CBD7F49E9BD"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-3</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:41:10 GMT</Last-Modified>
        <Etag>"0x8D46CBD8B243D68"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-4</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:41:25 GMT</Last-Modified>
        <Etag>"0x8D46CBD93FED46F"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
        </Properties>
      </Container>
      <Container>
        <Name>container-5</Name>
        <Properties>
          <Last-Modified>Thu, 16 Mar 2017 22:41:39 GMT</Last-Modified>
          <Etag>"0x8D46CBD9C762815"</Etag>
          <LeaseStatus>unlocked</LeaseStatus>
          <LeaseState>available</LeaseState>
        </Properties>
      </Container>
  </Containers>
  <NextMarker />
</EnumerationResults>
```

Artık isteği oluşturmayı, hizmeti çağırmayı ve sonuçları ayrıştırmayı öğrendiğinize göre, yetkilendirme üst bilgisini oluşturmayı görelim. Bu üst bilginin oluşturulması karmaşıktır, ancak her şey, kodun çalışır hale getirildikten sonra tüm depolama hizmeti REST API 'Lerinde çalışır.

## <a name="creating-the-authorization-header"></a>Yetkilendirme üst bilgisi oluşturuluyor

> [!TIP]
> Azure depolama artık Bloblar ve kuyruklar için Azure Active Directory (Azure AD) tümleştirmesini desteklemektedir. Azure AD, Azure depolama 'ya yönelik bir isteği yetkilendirmek için çok daha basit bir deneyim sunar. REST işlemlerini yetkilendirmek için Azure AD kullanma hakkında daha fazla bilgi için bkz. [Azure Active Directory Ile yetkilendirme](/rest/api/storageservices/authorize-with-azure-active-directory). Azure depolama ile Azure AD tümleştirmesi 'ne genel bakış için, bkz. [Azure Active Directory kullanarak Azure depolama 'ya erişim kimlik doğrulaması](storage-auth-aad.md).

[Azure depolama 'ya Istekleri yetkilendirmek](/rest/api/storageservices/authorize-requests-to-azure-storage)için kavramsal (kod yok) açıklayan bir makale vardır.

Bu makalenin tam olarak gerekli olduğunu ve kodu göstermesini görelim.

Önce, paylaşılan anahtar yetkilendirmesi kullanın. Yetkilendirme üst bilgisi biçimi şöyle görünür:

```  
Authorization="SharedKey <storage account name>:<signature>"  
```

İmza alanı, istekten oluşturulan, karma tabanlı bir İleti Kimlik Doğrulama Kodu (HMAC) ve SHA256 algoritması kullanılarak hesaplanarak Base64 kodlaması kullanılarak kodlanır. Bunun ne var? (Buradan da bekleyin, *kurallı* sözcüğünü henüz duymadınız.)

Bu kod parçacığı, paylaşılan anahtar imza dizesinin biçimini gösterir:

```csharp  
StringToSign = VERB + "\n" +  
               Content-Encoding + "\n" +  
               Content-Language + "\n" +  
               Content-Length + "\n" +  
               Content-MD5 + "\n" +  
               Content-Type + "\n" +  
               Date + "\n" +  
               If-Modified-Since + "\n" +  
               If-Match + "\n" +  
               If-None-Match + "\n" +  
               If-Unmodified-Since + "\n" +  
               Range + "\n" +  
               CanonicalizedHeaders +  
               CanonicalizedResource;  
```

Bu alanların çoğu nadiren kullanılır. BLOB depolama için, FIIL, MD5, içerik uzunluğu, kurallı üstbilgileri ve kurallı kaynağını belirtirsiniz. Diğerlerinin boş bırakabilmesini sağlayabilirsiniz (ancak `\n` bunları boş olarak bilir).

CanonicalizedHeaders ve CanonicalizedResource nedir? İyi soru. Aslında kurallı ne anlama geliyor? Microsoft Word bunu bir sözcük olarak tanımaz. Şu şekilde, bu durum, [kurallı kullanım hakkında diyor](https://en.wikipedia.org/wiki/Canonicalization): *Bilgisayar bilimi 'nde, kurallı kullanım (bazen standartlaştırma veya normalleştirme), birden fazla olası temsili olan verileri "standart", "normal" veya kurallı biçimde dönüştürmeye yönelik bir işlemdir.* Normal konuşurken bu, öğelerin listesini (örneğin, kurallı üst bilgileri gibi) almak ve bunları gerekli bir biçimde standartlaştırmanız anlamına gelir. Temel olarak, Microsoft bir biçimde karar vermiştir ve bunu değiştirmeniz gerekir.

Yetkilendirme üst bilgisini oluşturmak için gerektiğinden, bu iki kurallı alanı ile başlayalım.

**Kurallı üst bilgileri**

Bu değeri oluşturmak için "x-MS-" ile başlayan üst bilgileri alın ve bunları sıralayın, sonra bunları tek bir dizede birleştirilmiş bir `[key:value\n]` örnek dizesi olarak biçimlendirin. Bu örnekte, kurallı üstbilgileri şöyle görünür: 

```
x-ms-date:Fri, 17 Nov 2017 00:44:48 GMT\nx-ms-version:2017-07-29\n
```

Bu çıktıyı oluşturmak için kullanılan kod aşağıda verilmiştir:

```csharp 
private static string GetCanonicalizedHeaders(HttpRequestMessage httpRequestMessage)
{
    var headers = from kvp in httpRequestMessage.Headers
        where kvp.Key.StartsWith("x-ms-", StringComparison.OrdinalIgnoreCase)
        orderby kvp.Key
        select new { Key = kvp.Key.ToLowerInvariant(), kvp.Value };

    StringBuilder headersBuilder = new StringBuilder();

    // Create the string in the right format; this is what makes the headers "canonicalized" --
    //   it means put in a standard format. https://en.wikipedia.org/wiki/Canonicalization
    foreach (var kvp in headers)
    {
        headersBuilder.Append(kvp.Key);
        char separator = ':';

        // Get the value for each header, strip out \r\n if found, then append it with the key.
        foreach (string headerValue in kvp.Value)
        {
            string trimmedValue = headerValue.TrimStart().Replace("\r\n", string.Empty);
            headersBuilder.Append(separator).Append(trimmedValue);

            // Set this to a comma; this will only be used
            // if there are multiple values for one of the headers.
            separator = ',';
        }

        headersBuilder.Append("\n");
    }

    return headersBuilder.ToString();
}
```

**Kurallı kaynağı**

İmza dizesinin bu bölümü, isteğin hedeflediği depolama hesabını temsil eder. İstek URI 'sinin `<http://contosorest.blob.core.windows.net/?comp=list>`gerçek hesap adıyla (`contosorest` bu durumda) olduğunu unutmayın. Bu örnekte, bu döndürülür:

```
/contosorest/\ncomp:list
```

Sorgu parametreleriniz varsa bu örnek bu parametreleri de içerir. Ayrıca, ek sorgu parametrelerini ve birden çok değer içeren sorgu parametrelerini de işleyen kod aşağıda verilmiştir. Bu kodu, tüm REST API 'Leri için çalışacak şekilde oluşturduğunuzu unutmayın. ListContainers yönteminin tümüne ihtiyacı olmasa bile tüm olasılıkları dahil etmek istiyorsunuz.

```csharp
private static string GetCanonicalizedResource(Uri address, string storageAccountName)
{
    // The absolute path will be "/" because for we're getting a list of containers.
    StringBuilder sb = new StringBuilder("/").Append(storageAccountName).Append(address.AbsolutePath);

    // Address.Query is the resource, such as "?comp=list".
    // This ends up with a NameValueCollection with 1 entry having key=comp, value=list.
    // It will have more entries if you have more query parameters.
    NameValueCollection values = HttpUtility.ParseQueryString(address.Query);

    foreach (var item in values.AllKeys.OrderBy(k => k))
    {
        sb.Append('\n').Append(item).Append(':').Append(values[item]);
    }

    return sb.ToString().ToLower();
}
```

Artık kurallı dizeleri ayarlandığına göre, yetkilendirme üst bilgisinin nasıl oluşturulacağını inceleyelim. Bu makalede daha önce gösterildiği StringToSign biçiminde ileti imzasının bir dizesini oluşturarak başlayın. Bu kavram, koddaki açıklamaları kullanarak daha kolay açıklanmak, bu nedenle yetkilendirme üst bilgisini döndüren son yöntemdir:

```csharp
internal static AuthenticationHeaderValue GetAuthorizationHeader(
    string storageAccountName, string storageAccountKey, DateTime now,
    HttpRequestMessage httpRequestMessage, string ifMatch = "", string md5 = "")
{
    // This is the raw representation of the message signature.
    HttpMethod method = httpRequestMessage.Method;
    String MessageSignature = String.Format("{0}\n\n\n{1}\n{5}\n\n\n\n{2}\n\n\n\n{3}{4}",
                method.ToString(),
                (method == HttpMethod.Get || method == HttpMethod.Head) ? String.Empty
                  : httpRequestMessage.Content.Headers.ContentLength.ToString(),
                ifMatch,
                GetCanonicalizedHeaders(httpRequestMessage),
                GetCanonicalizedResource(httpRequestMessage.RequestUri, storageAccountName),
                md5);

    // Now turn it into a byte array.
    byte[] SignatureBytes = Encoding.UTF8.GetBytes(MessageSignature);

    // Create the HMACSHA256 version of the storage key.
    HMACSHA256 SHA256 = new HMACSHA256(Convert.FromBase64String(storageAccountKey));

    // Compute the hash of the SignatureBytes and convert it to a base64 string.
    string signature = Convert.ToBase64String(SHA256.ComputeHash(SignatureBytes));

    // This is the actual header that will be added to the list of request headers.
    AuthenticationHeaderValue authHV = new AuthenticationHeaderValue("SharedKey",
        storageAccountName + ":" + Convert.ToBase64String(SHA256.ComputeHash(SignatureBytes)));
    return authHV;
}
```

Bu kodu çalıştırdığınızda, elde edilen MessageSignature Şu örneğe benzer şekilde görünür:

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 01:07:37 GMT\nx-ms-version:2017-07-29\n/contosorest/\ncomp:list
```

AuthorizationHeader için son değer aşağıda verilmiştir:

```
SharedKey contosorest:Ms5sfwkA8nqTRw7Uury4MPHqM6Rj2nfgbYNvUKOa67w=
```

AuthorizationHeader, yanıtı göndermeden önce istek üst bilgilerine yerleştirilmiş son üst bilgi.

Bu, depolama hizmetleri REST API 'Lerini çağırmak için bir istek oluşturabileceğiniz bir sınıfı birlikte koymak için bilmeniz gereken her şeyi içerir.

## <a name="how-about-another-example"></a>Başka bir örnek nasıl? 

Bu kodun, kapsayıcı *kapsayıcısı-1*Için listblobları çağırmak üzere nasıl değiştirileceğini inceleyelim. Bu kod, kapsayıcıları listeleme koduyla neredeyse aynıdır, URI ve yanıtı nasıl ayrıştırdığınızda tek farklılık vardır. 

[Listbloblar](/rest/api/storageservices/List-Blobs)için başvuru belgelerine bakarsanız, yöntemin *alınacağını* ve RequestUri 'nin şu olduğunu fark edersiniz:

```
https://myaccount.blob.core.windows.net/container-1?restype=container&comp=list
```

ListContainersAsyncREST içinde, URL 'yi Listbloblar için API olarak ayarlayan kodu değiştirin. Kapsayıcı adı **Container-1**' dir.

```csharp
String uri = 
    string.Format("http://{0}.blob.core.windows.net/container-1?restype=container&comp=list",
      storageAccountName);

```

Yanıtı nerede işleyeceğinizi kullanırsanız, kodu kapsayıcı yerine Blobları aramak üzere değiştirin.

```csharp
foreach (XElement container in x.Element("Blobs").Elements("Blob"))
{
    Console.WriteLine("Blob name = {0}", container.Element("Name").Value);
}
```

Bu örneği çalıştırdığınızda aşağıdaki gibi sonuçlar alırsınız:

**Kurallı üst bilgileri:**

```
x-ms-date:Fri, 17 Nov 2017 05:16:48 GMT\nx-ms-version:2017-07-29\n
```

**Kurallı kaynağı:**

```
/contosorest/container-1\ncomp:list\nrestype:container
```

**MessageSignature:**

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 05:16:48 GMT
  \nx-ms-version:2017-07-29\n/contosorest/container-1\ncomp:list\nrestype:container
```

**AuthorizationHeader:**

```
SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
```

Aşağıdaki değerler [Fiddler](https://www.telerik.com/fiddler)'dan alınır:

**İsteyen**

```
GET http://contosorest.blob.core.windows.net/container-1?restype=container&comp=list HTTP/1.1
```

**İstek üst bilgileri:**

```
x-ms-date: Fri, 17 Nov 2017 05:16:48 GMT
x-ms-version: 2017-07-29
Authorization: SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Yürütme sonrasında döndürülen durum kodu ve yanıt üstbilgileri:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 7e9316da-001e-0037-4063-5faf9d000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 05:20:21 GMT
Content-Length: 1135
```

**Yanıt gövdesi (XML):** Bu XML yanıtı, Blobların ve bunların özelliklerinin listesini gösterir. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<EnumerationResults 
    ServiceEndpoint="http://contosorest.blob.core.windows.net/" ContainerName="container-1">
    <Blobs>
        <Blob>
            <Name>DogInCatTree.png</Name>
            <Properties><Last-Modified>Fri, 17 Nov 2017 01:41:14 GMT</Last-Modified>
            <Etag>0x8D52D5C4A4C96B0</Etag>
            <Content-Length>419416</Content-Length>
            <Content-Type>image/png</Content-Type>
            <Content-Encoding />
            <Content-Language />
            <Content-MD5 />
            <Cache-Control />
            <Content-Disposition />
            <BlobType>BlockBlob</BlobType>
            <LeaseStatus>unlocked</LeaseStatus>
            <LeaseState>available</LeaseState>
            <ServerEncrypted>true</ServerEncrypted>
            </Properties>
        </Blob>
        <Blob>
            <Name>GuyEyeingOreos.png</Name>
            <Properties>
                <Last-Modified>Fri, 17 Nov 2017 01:41:14 GMT</Last-Modified>
                <Etag>0x8D52D5C4A25A6F6</Etag>
                <Content-Length>167464</Content-Length>
                <Content-Type>image/png</Content-Type>
                <Content-Encoding />
                <Content-Language />
                <Content-MD5 />
                <Cache-Control />
                <Content-Disposition />
                <BlobType>BlockBlob</BlobType>
                <LeaseStatus>unlocked</LeaseStatus>
                <LeaseState>available</LeaseState>
                <ServerEncrypted>true</ServerEncrypted>
            </Properties>
            </Blob>
        </Blobs>
    <NextMarker />
</EnumerationResults>
```

## <a name="summary"></a>Özet

Bu makalede, BLOB depolama REST API istek yapmayı öğrendiniz. İstek ile, kapsayıcıların bir listesini veya bir kapsayıcıdaki Blobların listesini alabilirsiniz. REST API çağrısı için yetkilendirme imzasının nasıl oluşturulacağını ve bunu REST isteğinde nasıl kullanacağınızı öğrendiniz. Son olarak, yanıtı İnceleme hakkında daha fazla öğrendiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Blob hizmeti REST API](/rest/api/storageservices/blob-service-rest-api)
* [Dosya hizmeti REST API](/rest/api/storageservices/file-service-rest-api)
* [Kuyruk hizmeti REST API](/rest/api/storageservices/queue-service-rest-api)
* [Tablo hizmeti REST API](/rest/api/storageservices/table-service-rest-api)
