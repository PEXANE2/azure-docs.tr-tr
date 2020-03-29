---
title: Paylaşılan Anahtar yetkilendirmesi ile REST API operasyonlarını arayın
titleSuffix: Azure Storage
description: Paylaşılan Anahtar yetkilendirmesini kullanarak Blob depolamasına istekte bulunmak için Azure Depolama REST API'sini kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: f5c6125b850062450516e7fc0b19c2e0d5d6f577
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77916073"
---
# <a name="call-rest-api-operations-with-shared-key-authorization"></a>Paylaşılan Anahtar yetkilendirmesi ile REST API operasyonlarını arayın

Bu makalede, Yetkilendirme üstbilgisini nasıl oluşturabileceğiniz de dahil olmak üzere Azure Depolama REST API'lerini nasıl arayacağınız gösterilmektedir. Rest hakkında hiçbir şey bilmeyen ve REST araması yapmak için hiçbir fikrim olmayan bir geliştiricinin bakış açısından yazılmıştır. REST işlemini nasıl çağırabileceğinizi öğrendikten sonra, diğer Azure Depolama REST işlemlerini kullanmak için bu bilgiden yararlanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Örnek uygulama, bir depolama hesabı için blob kapsayıcılarını listeler. Bu makaledeki kodu denemek için aşağıdaki öğelere ihtiyacınız vardır:

- **Azure geliştirme** iş yüküyle Visual [Studio 2019'u](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) yükleyin.

- Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

- Genel amaçlı bir depolama hesabı. Henüz bir depolama hesabınız yoksa, [bkz.](storage-account-create.md)

- Bu makaledeki örnek, bir depolama hesabında kapsayıcıların nasıl listelenebildiğini gösterir. Çıktıyı görmek için, başlamadan önce depolama hesabındaki blob depolama alanına bazı kapsayıcılar ekleyin.

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Örnek uygulama C# ile yazılmış bir konsol uygulamasıdır.

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın.

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Visual Studio çözümünü açmak için storage-dotnet-rest-api-with-auth klasörünü arayın, açın ve StorageRestApiAuth.sln'ye çift tıklayın.

## <a name="about-rest"></a>REST Hakkında

REST *temsili durum transferi*anlamına gelir. Belirli bir tanım [için, Vikipedi'ye](https://en.wikipedia.org/wiki/Representational_state_transfer)göz atın.

REST, http/HTTPS gibi bir internet protokolü üzerinden bir hizmetle etkileşim kurmanızı sağlayan bir mimaridir. REST, sunucuda veya istemcide çalışan yazılımdan bağımsızdır. REST API'si HTTP/HTTPS'yi destekleyen herhangi bir platformdan çağrılabilir. Mac, Windows, Linux, Android telefon veya tablet, iPhone, iPod veya web sitesinde çalışan bir uygulama yazabilir ve tüm bu platformlar için aynı REST API'sını kullanabilirsiniz.

REST API'ye yapılan bir çağrı, istemci tarafından yapılan bir istek ve hizmet tarafından döndürülen bir yanıttan oluşur. İstekte, hangi işlemi aramak istediğiniz, harekete geçilebilmek için kaynak, sorgu parametreleri ve üstbilgiler ve çağrılan işlem, bir veri yüküne bağlı olarak bilgi içeren bir URL gönderirsiniz. Hizmetten gelen yanıt, bir durum kodu, bir yanıt üstbilgileri kümesi ve çağrılan işlem, bir veri yükü ne bağlı olarak içerir.

## <a name="about-the-sample-application"></a>Örnek uygulama hakkında

Örnek uygulama, bir depolama hesabındaki kapsayıcıları listeler. REST API belgelerindeki bilgilerin gerçek kodunuzla nasıl ilişkili olduğunu anladıktan sonra, diğer REST aramalarını anlamak daha kolaydır.

[Blob Service REST API'ye](/rest/api/storageservices/Blob-Service-REST-API)bakarsanız, blob depolamada gerçekleştirebileceğiniz tüm işlemleri görürsünüz. Depolama istemcisi kitaplıkları REST API'lerinin etrafındaki sarmalayıcılardır – rest API'lerini doğrudan kullanmadan depolamaya erişmenizi kolaylaştırır. Ancak yukarıda belirtildiği gibi, bazen depolama istemcisi kitaplığı yerine REST API'yi kullanmak istersiniz.

## <a name="list-containers-operation"></a>Kapsayıcılar işlemini listele

[ListContainers](/rest/api/storageservices/List-Containers2) işlemi için başvuruyu gözden geçirin. Bu bilgiler, bazı alanların istek ve yanıtta nereden geldiğini anlamanıza yardımcı olur.

**İstek Yöntemi**: GET. Bu fiil, istek nesnesinin özelliği olarak belirttiğiniz HTTP yöntemidir. Bu fiiliçin diğer değerler, aradığınız API'ye bağlı olarak HEAD, PUT ve DELETE'i içerir.

**İstek**URI `https://myaccount.blob.core.windows.net/?comp=list`: .İstek URI blob depolama hesabı bitiş `http://myaccount.blob.core.windows.net` noktası ve `/?comp=list`kaynak dizesi oluşturulur.

[URI parametreleri](/rest/api/storageservices/List-Containers2#uri-parameters): ListContainers'ı ararken kullanabileceğiniz ek sorgu parametreleri vardır. Bu parametrelerden birkaçı, filtreleme için kullanılan arama (saniye cinsinden) ve *öneki*için zaman aşımlarıdır. *timeout*

Başka bir yararlı parametre *maxresults:* Bu değerden daha fazla kapsayıcı varsa, yanıt gövdesi sonraki istekte dönmek için bir sonraki kapsayıcıyı gösteren bir *NextMarker* öğesi içerir. Bu özelliği kullanmak için, bir sonraki isteği yaptığınızda URI'de *işaretleyici* parametresi olarak *NextMarker* değerini sağlarsınız. Bu özelliği kullanırken, sonuçları sayfalama benzer.

Ek parametrelerkullanmak için, aşağıdaki örnekgibi değeri olan kaynak dizesine ekleyin:

```
/?comp=list&timeout=60&maxresults=100
```

[İstek Üstbilgi :](/rest/api/storageservices/List-Containers2#request-headers)**Bu** bölümde gerekli ve isteğe bağlı istek üstbilgisi listelemektedir. Üstbilgiden üçü gereklidir: *Yetkilendirme* üstbilgisi, *x-ms tarihi* (istek için UTC saatini içerir) ve *x-ms sürümü* (REST API'nin kullanım sürümünü belirtir). Üstbilgide *x-ms-istemci-istek-id* dahil olmak üzere isteğe bağlıdır – bu alanın değerini herhangi bir şeye ayarlayabilirsiniz; günlüğe kaydetme etkinleştirildiğinde depolama analizi günlüklerine yazılır.

[İstek Gövdesi](/rest/api/storageservices/List-Containers2#request-body)**:** ListContainers için istek gövdesi yoktur. İstek Gövdesi, blobs yüklerken PUT işlemlerinin tümünün yanı sıra, uygulanacak depolanan erişim ilkelerinin XML listesini göndermenize olanak tanıyan SetContainerAccessPolicy'de kullanılır. Paylaşılan [Erişim İmzalarını (SAS) kullanma](storage-sas-overview.md)makalesinde depolanan erişim ilkeleri tartışılır.

[Yanıt Durum Kodu](/rest/api/storageservices/List-Containers2#status-code)**:** Bilmeniz gereken durum kodlarını bildirir. Bu örnekte, 200'lük bir HTTP durum kodu tamamdır. HTTP durum kodlarının tam listesi için [Durum Kodu Tanımlarına](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)göz atın. Depolama REST API'lerine özgü hata kodlarını görmek için [Bkz.](/rest/api/storageservices/common-rest-api-error-codes)

[Yanıt Üstleri](/rest/api/storageservices/List-Containers2#response-headers)**:** Bunlar *İçerik Türünü*içerir; *x-ms-request-id*, içinde geçtiğiniz istek kimliği; *x-ms sürümü,* kullanılan Blob hizmetinin sürümünü gösterir; ve UTC'de bulunan ve isteğin ne zaman yapıldığını gösteren *Tarih.*

[Yanıt Gövdesi](/rest/api/storageservices/List-Containers2#response-body): Bu alan istenen verileri sağlayan bir XML yapısıdır. Bu örnekte, yanıt kapsayıcıların ve özelliklerinin bir listesidir.

## <a name="creating-the-rest-request"></a>REST isteğini oluşturma

Üretimde çalışırken güvenlik için her zaman HTTP yerine HTTPS kullanın. Bu alıştırmanın amaçları için, istek ve yanıt verilerini görüntüleyebilmeniz için HTTP'yi kullanmalısınız. Gerçek REST aramalarında istek ve yanıt bilgilerini görüntülemek için [Fiddler'ı](https://www.telerik.com/fiddler) veya benzer bir uygulamayı indirebilirsiniz. Visual Studio çözümünde, depolama hesabı adı ve anahtarı sınıfta kodlanır. ListContainersAsyncREST yöntemi, DEPOLAMA hesabı adı ve depolama hesabı anahtarını REST isteğinin çeşitli bileşenlerini oluşturmak için kullanılan yöntemlere geçirir. Gerçek bir uygulamada, depolama hesabı adı ve anahtarı bir yapılandırma dosyasında, ortam değişkenlerinde veya Azure Anahtar Kasasından alınabilir.

Örnek projemizde, Yetkilendirme üstbilgisi oluşturma kodu ayrı bir sınıftadır. Fikir, tüm sınıf alabilir ve kendi çözüm ekleyebilirsiniz ve "olduğu gibi." Yetkilendirme üstbilgi kodu, Azure Depolama'ya yapılan REST API çağrılarının çoğunda çalışır.

Bir HttpRequestMessage nesnesi olan isteği oluşturmak için Program.cs'da ListContainersAsyncREST'e gidin. İsteğin oluşturulmasına yönelik adımlar şunlardır:

- Hizmeti aramak için kullanılacak URI'yi oluşturun.
- HttpRequestMessage nesnesini oluşturun ve yükü ayarlayın. Loadload ListContainersAsyncREST için geçersizçünkü biz bir şey geçmiyor.
- X-ms-date ve x-ms-version için istek üstaylarını ekleyin.
- Yetkilendirme üstbilgisini alın ve ekleyin.

İhtiyacınız olan bazı temel bilgiler:

- ListContainers için **yöntem.** `GET` Bu değer, istek anında ayarlanır.
- **Kaynak,** URI'nin hangi API'nin çağrıldığını gösteren sorgu bölümüdür, `/?comp=list`bu nedenle değer . Daha önce de belirtildiği gibi, kaynak [ListContainers API](/rest/api/storageservices/List-Containers2)hakkında bilgileri gösteren başvuru belgeleri sayfasında.
- URI, bu depolama hesabı için Blob hizmet bitiş noktası oluşturularak ve kaynağı nidarlayarak oluşturulur. **İstek URI** için değer `http://contosorest.blob.core.windows.net/?comp=list`olmak biter .
- ListContainers için, **requestBody** null ve hiçbir ekstra **üstbilgi**vardır.

Farklı API'ler gibi geçmek için diğer parametreler olabilir *ifMatch*. IfMatch'i putblob'u ararken kullanabileceğiniz bir örnek. Bu durumda, ifMatch'i bir eTag olarak ayarlarsınız ve sağladığınız eTag blob'daki geçerli eTag ile eşleşirse yalnızca blob'u güncelleştirir. ETag'ı geri aldığından beri başka biri blob'u güncellediyse, değişikliği geçersiz kılınır.

İlk olarak, `uri` ayarlayın `payload`ve .

```csharp
// Construct the URI. It will look like this:
//   https://myaccount.blob.core.windows.net/resource
String uri = string.Format("http://{0}.blob.core.windows.net?comp=list", storageAccountName);

// Provide the appropriate payload, in this case null.
//   we're not passing anything in.
Byte[] requestPayload = null;
```

Ardından, yöntemi ayarlayarak ve URI'yi `GET` sağlayarak isteği anında belirleyin.

```csharp
// Instantiate the request message with a null payload.
using (var httpRequestMessage = new HttpRequestMessage(HttpMethod.Get, uri)
{ Content = (requestPayload == null) ? null : new ByteArrayContent(requestPayload) })
{
```

İstek üstbilgisini `x-ms-date` ekleyin `x-ms-version`ve . Koddaki bu yer, arama için gereken ek istek üstbilgilerini de eklediğiniz yerdir. Bu örnekte, ek üstbilgi yoktur. Ek üstbilgide geçen bir API örneği, Set Container ACL işlemidir. Bu API çağrısı, "x-ms-blob-public-access" adlı bir üstbilgi ve erişim düzeyi için değer ekler.

```csharp
// Add the request headers for x-ms-date and x-ms-version.
DateTime now = DateTime.UtcNow;
httpRequestMessage.Headers.Add("x-ms-date", now.ToString("R", CultureInfo.InvariantCulture));
httpRequestMessage.Headers.Add("x-ms-version", "2017-07-29");
// If you need any additional headers, add them here before creating
//   the authorization header.
```

Yetkilendirme üstbilgisini oluşturan yöntemi çağırın ve istek üstbilgisine ekleyin. Yetkilendirme üstbilgisini daha sonra makalede nasıl oluşturacağınız üzerinde görürsünüz. Yöntem adı, bu kod parçacığında görebileceğiniz GetAuthorizationHeader'dır:

```csharp
// Get the authorization header and add it.
httpRequestMessage.Headers.Authorization = AzureStorageAuthenticationHelper.GetAuthorizationHeader(
    storageAccountName, storageAccountKey, now, httpRequestMessage);
```

Bu noktada, `httpRequestMessage` yetkilendirme üstbilgileriyle birlikte tamamlanan REST isteğini içerir.

## <a name="send-the-request"></a>İsteği gönderme

İsteği siz inşa ettiğinize göre, GönderAsync yöntemini arayarak Azure Depolama'ya gönderebilirsiniz. Yanıt durum kodunun değerinin 200 olup olmadığını, yani işlemin başarılı olduğunu denetleyin. Sonra, yanıtı ayrışdırın. Bu durumda, kapsayıcıların bir XML listesini alırsınız. İstek oluşturmak, isteği yürütmek ve sonra kapsayıcılar listesi için yanıtı incelemek için GetRESTRequest yöntemini arama koduna bakalım.

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

SendAsync'i ararken [Fiddler](https://www.telerik.com/fiddler) gibi bir ağ algılayıcısı çalıştırıyorsanız, istek ve yanıt bilgilerini görebilirsiniz. Bir bakalım. Depolama hesabının adı *contosorest*olduğunu.

**Istek:**

```
GET /?comp=list HTTP/1.1
```

**İstek Başlıkları:**

```
x-ms-date: Thu, 16 Nov 2017 23:34:04 GMT
x-ms-version: 2014-02-14
Authorization: SharedKey contosorest:1dVlYJWWJAOSHTCPGiwdX1rOS8B4fenYP/VrU0LfzQk=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Durum kodu ve yanıt üstbilgiyürütmeden sonra döndürülür:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 3e889876-001e-0039-6a3a-5f4396000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 00:23:42 GMT
Content-Length: 1511
```

**Yanıt gövdesi (XML):** Liste Kapsayıcıları işlemi için bu, kapsayıcıların listesini ve özelliklerini gösterir.

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

Artık isteği nasıl oluşturabileceğinizi, hizmeti nasıl arayacağınızı ve sonuçları ayrıştırmayı anladığınıza göre, yetkilendirme üstbilgisini nasıl oluşturabileceğinizi görelim. Bu üstbilgi oluşturma karmaşıktır, ancak iyi haber, bir kez kod çalışma var, tüm Depolama Hizmeti REST API'lar için çalışır.

## <a name="creating-the-authorization-header"></a>Yetkilendirme üstbilgisini oluşturma

> [!TIP]
> Azure Depolama artık blob'lar ve kuyruklar için Azure Active Directory (Azure AD) tümleştirmesi'ni destekliyor. Azure AD, Azure Depolama'ya bir isteği yetkilendirmek için çok daha basit bir deneyim sunar. REST işlemlerini yetkilendirmek için Azure AD'yi kullanma hakkında daha fazla bilgi için Azure [Etkin Dizinile Yetkilendirme'ye](/rest/api/storageservices/authorize-with-azure-active-directory)bakın. Azure Depolama ile Azure AD entegrasyonuna genel bakış için Azure [Etkin Dizini kullanarak Azure Depolama'ya kimlik doğrulama erişimi](storage-auth-aad.md)bölümüne bakın.

Azure Depolama'ya isteklerin nasıl [yetkiverişilen](/rest/api/storageservices/authorize-requests-to-azure-storage)kavramsal olarak (kod yok) açıklayan bir makale vardır.

Bu makaleyi tam olarak gerekli olana kadar damıtalım ve kodu gösterelim.

İlk olarak, Paylaşılan Anahtar yetkilendirmesi kullanın. Yetkilendirme üstbilgi biçimi aşağıdaki gibi görünür:

```  
Authorization="SharedKey <storage account name>:<signature>"  
```

İmza alanı, istekten oluşturulan ve SHA256 algoritması kullanılarak hesaplanan ve base64 kodlaması kullanılarak kodlanan Karma tabanlı İleti Kimlik Doğrulama Kodudur (HMAC). Anladın mı? (Orada hang, hatta kelime *kanoniize* duymadım henüz.)

Bu kod snippet Paylaşılan Anahtar imza dizesinin biçimini gösterir:

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

Bu alanların çoğu nadiren kullanılır. Blob depolama için VERB, md5, içerik uzunluğu, Canonicalized Üstbilgileri ve Canonicalized Kaynak belirtin. Diğerlerini boş bırakabilirsiniz (ancak `\n` boş olduklarını bilmesi için içine alabilirsiniz).

CanonicalizedHeaders ve CanonicalizedResource nedir? Güzel soru. Aslında, kanonikleştirilmiş ne demek? Microsoft Word bile bir kelime olarak tanımıyor. Vikipedi'nin [kanonikleştirme hakkında söyledikleri](https://en.wikipedia.org/wiki/Canonicalization)şunlardır: Bilgisayar *biliminde, kanoniizasyon (bazen standardizasyon veya normalleştirme) birden fazla olası temsili olan verileri "standart", "normal" veya kanonik biçime dönüştürmek için bir süreçtir.* Normal konuşmada, bu öğelerin listesini almak (Canonicalized Üstbilgi durumunda üstbilgi gibi) ve gerekli bir biçimde standartlaştırmak anlamına gelir. Temel olarak, Microsoft bir biçim üzerinde karar verdi ve bunu eşleştirmek gerekir.

Yetkilendirme üstbilgisini oluşturmaları gerektiğinden, bu iki kanonik leştirilmiş alanla başlayalım.

### <a name="canonicalized-headers"></a>Kanonik leştirilmiş üstbilgi

Bu değeri oluşturmak için, "x-ms-" ile başlayan başlıkları alın ve sıralayın, `[key:value\n]` ardından bunları bir dizi örnek halinde biçimlendirin, tek bir dize içine sıkıştırılmış. Bu örnekte, standartlaştırılmış üstbilgi aşağıdaki gibi görünür:

```
x-ms-date:Fri, 17 Nov 2017 00:44:48 GMT\nx-ms-version:2017-07-29\n
```

Bu çıktıyı oluşturmak için kullanılan kod aşağıda vereb:

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

### <a name="canonicalized-resource"></a>Kanonik leştirilmiş kaynak

İmza dizesinin bu bölümü, istek tarafından hedeflenen depolama hesabını temsil eder. İstek URI'nin `<http://contosorest.blob.core.windows.net/?comp=list>`gerçek hesap adı (bu`contosorest` durumda) olduğunu unutmayın. Bu örnekte, bu döndürülür:

```
/contosorest/\ncomp:list
```

Sorgu parametrelerinvarsa, bu örnek de bu parametreleri içerir. Ek sorgu parametrelerini ve birden çok değeriçeren sorgu parametrelerini de işleyen kod buradadır. Tüm REST API'leri için çalışmak üzere bu kodu oluşturuyorsunuz unutmayın. ListContainers yönteminin tümüne gerek duymasa bile tüm olasılıkları eklemek istiyorsunuz.

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
        sb.Append('\n').Append(item.ToLower()).Append(':').Append(values[item]);
    }

    return sb.ToString();
}
```

Artık kanonize dizeleri ayarlanmış olduğundan, yetkilendirme üstbilgisinin kendisini nasıl oluşturabileceğinize bakalım. Daha önce bu makalede görüntülenen StringToSign biçiminde ileti imzası bir dize oluşturarak başlar. Bu kavramı koddaki açıklamaları kullanarak açıklamak daha kolaydır, bu nedenle burada, Yetkilendirme Üstbilgi döndürür son yöntem:

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
        storageAccountName + ":" + signature);
    return authHV;
}
```

Bu kodu çalıştırdığınızda, ortaya çıkan İleti İmzası aşağıdaki örnek gibi görünür:

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 01:07:37 GMT\nx-ms-version:2017-07-29\n/contosorest/\ncomp:list
```

Yetkilendirme Header için son değer şunlardır:

```
SharedKey contosorest:Ms5sfwkA8nqTRw7Uury4MPHqM6Rj2nfgbYNvUKOa67w=
```

Yetkilendirme Header yanıtı göndermeden önce istek üstbilgisine yerleştirilen son üstbilgidir.

Bu, Depolama Hizmetleri REST API'lerini aramak için bir istek oluşturabileceğiniz bir sınıf oluşturmak için bilmeniz gereken her şeyi kapsar.

## <a name="example-list-blobs"></a>Örnek: Liste lekeleri

*Kapsayıcı-1*için Liste Blobs işlemi aramak için kodu değiştirmek için nasıl bakalım. Bu kod, liste kapsayıcıları için kod hemen hemen aynıdır, tek farkURI ve nasıl yanıt ayrıştırmak olmak.

[ListBlobs](/rest/api/storageservices/List-Blobs)için referans belgelerine bakarsanız, yöntemin *GET* ve RequestURI olduğunu bulabilirsiniz:

```
https://myaccount.blob.core.windows.net/container-1?restype=container&comp=list
```

ListContainersAsyncREST'de, URI'yi ListBlobs için API olarak ayarlayan kodu değiştirin. Konteyner adı **konteyner-1.**

```csharp
String uri =
    string.Format("http://{0}.blob.core.windows.net/container-1?restype=container&comp=list",
      storageAccountName);

```

Ardından yanıtı işlediğiniz yerde, kapsayıcılar yerine blobs aramak için kodu değiştirin.

```csharp
foreach (XElement container in x.Element("Blobs").Elements("Blob"))
{
    Console.WriteLine("Blob name = {0}", container.Element("Name").Value);
}
```

Bu örneği çalıştırdığınızda, aşağıdaki gibi sonuçlar alırsınız:

**Kanonik leştirilmiş üstbilgi:**

```
x-ms-date:Fri, 17 Nov 2017 05:16:48 GMT\nx-ms-version:2017-07-29\n
```

**Kanonik leştirilmiş kaynak:**

```
/contosorest/container-1\ncomp:list\nrestype:container
```

**İleti imzası:**

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 05:16:48 GMT
  \nx-ms-version:2017-07-29\n/contosorest/container-1\ncomp:list\nrestype:container
```

**Yetkilendirme üstbilgi:**

```
SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
```

Aşağıdaki değerler [Fiddler'dan:](https://www.telerik.com/fiddler)

**Istek:**

```
GET http://contosorest.blob.core.windows.net/container-1?restype=container&comp=list HTTP/1.1
```

**İstek Başlıkları:**

```
x-ms-date: Fri, 17 Nov 2017 05:16:48 GMT
x-ms-version: 2017-07-29
Authorization: SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Durum kodu ve yanıt üstbilgiyürütmeden sonra döndürülür:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 7e9316da-001e-0037-4063-5faf9d000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 05:20:21 GMT
Content-Length: 1135
```

**Yanıt gövdesi (XML):** Bu XML yanıtı, lekelerin listesini ve özelliklerini gösterir.

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

Bu makalede, blob depolama REST API için bir istek yapmayı öğrendi. İstekle birlikte, kapsayıcıların veya bir kapsayıcıdaki lekelerin listesini alabilirsiniz. REST API çağrısı için yetkilendirme imzasını nasıl oluşturabileceğinizi ve REST isteğinde nasıl kullanılacağını öğrendiniz. Son olarak, yanıtı incelemeyi öğrendin.

## <a name="next-steps"></a>Sonraki adımlar

- [Blob Hizmeti REST API'si](/rest/api/storageservices/blob-service-rest-api)
- [Dosya Hizmeti REST API'si](/rest/api/storageservices/file-service-rest-api)
- [Kuyruk Hizmeti REST API'si](/rest/api/storageservices/queue-service-rest-api)
- [Tablo Hizmeti REST API'si](/rest/api/storageservices/table-service-rest-api)
