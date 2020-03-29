---
title: REST | Microsoft Dokümanlar
description: Bu öğretici, REST API'yi kullanarak Azure Media Services ile isteğe bağlı içerik teslim uygulaması uygulama adımlarında size yol göstersin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 88194b59-e479-43ac-b179-af4f295e3780
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 8989acc6d21a3c53be9d97c74ed7fbf03ba54819
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773675"
---
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a>REST'i kullanarak isteğe bağlı içerik sunmaya başlayın  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Bu hızlı başlangıç, Azure Medya Hizmetleri (AMS) REST API'lerini kullanarak Isteğe Bağlı Video (VoD) içerik teslim uygulamasını uygulama adımlarında size yol göstersin.

Öğretici, temel Media Services iş akışını ve Media Services geliştirmek için gereken en genel programlama nesnelerini ve görevleri tanıtır. Öğreticinin tamamlanmasından sonra, yüklediğiniz, kodladığınız ve indirdiğiniz örnek bir ortam dosyasını akışla yükleyebilir veya aşamalı olarak indirebilirsiniz.

Aşağıdaki resimde Media Services OData modeliyle VoD uygulamaları geliştirirken en sık kullanılan nesnelerin bazıları gösterilmektedir.

Resmi tam boyutlu görüntülemek için tıklayın.  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a>Ön koşullar
REST API'leri ile Medya Hizmetleri ile geliştirmeye başlamak için aşağıdaki ön koşullar gereklidir.

* Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Bir Media Services hesabı. Bir Media Services hesabı oluşturmak için bkz. [Media Services hesabı oluşturma](media-services-portal-create-account.md).
* Medya Hizmetleri REST API ile nasıl geliştirileceğimiz hakkında anlayış. Daha fazla bilgi için Medya [Hizmetleri REST API genel bakış](media-services-rest-how-to-use.md)bakın.
* HTTP isteklerini ve yanıtlarını gönderebilen seçtiğiniz bir uygulama. Bu öğretici [Fiddler](https://www.telerik.com/download/fiddler)kullanır.

Aşağıdaki görevler bu hızlı başlatmada gösterilir.

1. Akış uç noktalarını başlatın (Azure portalını kullanarak).
2. REST API ile Medya Hizmetleri hesabına bağlanın.
3. Yeni bir varlık oluşturun ve REST API ile bir video dosyası yükleyin.
4. Kaynak dosyayı REST API ile uyarlanabilir bitrate MP4 dosyaları kümesine kodlayın.
5. Varlığı yayımlayın ve REST API ile akış ve aşamalı indirme URL'leri alın.
6. İçeriğinizi oynatın.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Her zaman aynı gün / erişim izinleri kullanıyorsanız, örneğin, uzun bir süre (yükleme olmayan ilkeler) yerinde kalması amaçlanan bulucular için ilkeler kullanıyorsanız aynı ilke kimliği kullanın. Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

Bu makalede kullanılan AMS REST varlıkları hakkında ayrıntılı bilgi için Azure [Medya Hizmetleri REST API Başvurusu'na](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)bakın. Ayrıca, bkz. [Azure Medya Hizmetleri kavramları.](media-services-concepts.md)

>[!NOTE]
>Medya Hizmetleri'ndeki varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri belirlemeniz gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API Geliştirme kurulumu'na](media-services-rest-how-to-use.md)bakın.

## <a name="start-streaming-endpoints-using-the-azure-portal"></a>Azure portal ile akış uç noktalarını başlatma

Azure Medya Hizmetleri ile çalışırken, en yaygın senaryolardan biri uyarlanabilir bithızı akışı üzerinden video sunmaktır. Media Services, bu akış biçimlerinin her birinin önceden paketlenmiş sürümlerini depolamanıza gerek kalmadan, uyarlamalı bit hızı MP4 ile kodlanmış içeriğinizi Media Services tarafından desteklenen akış biçimlerinde (MPEG DASH, HLS, Kesintisiz Akış) tam vaktinde göndermenize olanak tanıyan dinamik paketleme özelliğine sahiptir.

>[!NOTE]
>AMS hesabınız **oluşturulduğunda,** **Durduruldu** durumunda hesabınıza varsayılan akış bitiş noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir.

Akış uç noktasını başlatmak için aşağıdakileri yapın:

1. [Azure portal](https://portal.azure.com/)’da oturum açın.
2. Ayarlar penceresinde, Akış uç noktaları'na tıklayın.
3. Varsayılan akış uç noktasına tıklayın.

    VARSAYILAN AKIŞ UÇ NOKTASI AYRINTILARI penceresi görüntülenir.

4. Başlat simgesine tıklayın.
5. Yaptığınız değişiklikleri kaydetmek için Kaydet düğmesine tıklayın.

## <a name="connect-to-the-media-services-account-with-rest-api"></a><a id="connect"></a>REST API ile Medya Hizmetleri hesabına bağlanın

AMS API'sine nasıl bağlanabileceğiniz hakkında bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

## <a name="create-a-new-asset-and-upload-a-video-file-with-rest-api"></a><a id="upload"></a>Rest API ile yeni bir varlık oluşturun ve bir video dosyası yükleyin

Media Services’de dijital dosyalar bir varlığa yüklenir. **Varlık** varlığı video, ses, görüntü, küçük resim koleksiyonları, metin parçaları ve kapalı altyazı dosyaları (ve bu dosyalarla ilgili meta veriler) içerebilir.  Dosyalar varlığa yüklendikten sonra, içeriğiniz daha fazla işlem ve akış için bulutta güvenli bir şekilde saklanır.

Bir varlık oluştururken sağlamanız gereken değerlerden biri varlık oluşturma seçenekleridir. **Seçenekler** özelliği, bir Kıymetin oluşturulabileceği şifreleme seçeneklerini açıklayan bir numaralandırma değeridir. Geçerli bir değer, aşağıdaki listedeki değerlerden biridir, bu listedeki değerlerin bir birleşimi değildir:

* **Yok** = **0** - Şifreleme kullanılmaz. Bu seçeneği kullanırken içeriğiniz taşıma sırasında veya depolama alanında korumalı değildir.
    Aşamalı indirme kullanarak bir MP4 iletmeyi planlıyorsanız bu seçeneği kullanın.
* **StorageEncrypted** = **1** - AES-256 bit şifrelemekullanarak net içeriğinizi yerel olarak şifreler ve ardından güvenli bir şekilde şifrelendiği Azure Depolama'ya yükler. Depolama Şifrelemesi ile korunan varlıklar, kodlamadan önce otomatik olarak şifrelenerek şifrelenmiş bir dosya sistemine yerleştirilir ve yeni bir çıktı varlığı şeklinde geri yüklenmeden önce isteğe bağlı olarak yeniden şifrelenir. Depolama Şifrelemesinin birincil kullanım nedeni, yüksek kaliteli girdi medya dosyalarınızın güvenliğini güçlü şifrelemeyle diskte bekleyen konumda sağlamak istediğiniz durumdur.
* **CommonEncryptionProtected** = **2** - Daha önce şifrelenmiş ve Ortak Şifreleme veya PlayReady DRM ile korunmuş içerik yüklüyorsanız bu seçeneği kullanın (örneğin, PlayReady DRM ile korunan Sorunsuz Akış).
* **EnvelopeEncryptionProtected** = **4** – AES ile şifrelenmiş HLS yüklüyorsanız bu seçeneği kullanın. Dosyalar Transform Manager tarafından kodlanmış ve şifrelenmiş olmalıdır.

### <a name="create-an-asset"></a>Varlık oluşturma
Varlık, Video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı altyazı dosyaları da dahil olmak üzere Medya Hizmetleri'ndeki birden çok nesne türü veya kümeiçin bir kapsayıcıdır. REST API'de, bir Varlık oluşturmak için Medya Hizmetlerine POSTA isteği gönderilmesi ve kıymetinizle ilgili tüm özellik bilgilerini istek gövdesine yerleştirmeniz gerekmektedir.

Aşağıdaki örnekte, bir varlığın nasıl oluşturulabildiğini gösterilmektedir.

**HTTP İsteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45

    {"Name":"BigBuckBunny.mp4", "Options":"0"}


**HTTP Yanıt**

Başarılı olursa, aşağıdaki döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a>Varlık Dosyası Oluşturma
[AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) varlığı, bir blob kapsayıcısında depolanan bir video veya ses dosyasını temsil eder. Kıymet dosyası her zaman bir varlıkla ilişkilidir ve bir varlık bir veya birden çok Varlık Dosyası içerebilir. Bir varlık dosyası nesnesi bir blob kapsayıcısındaki dijital bir dosyayla ilişkilendirilmezse, Medya Hizmetleri Encoder görevi başarısız olur.

Dijital medya dosyanızı bir blob kapsayıcısına yükledikten sonra, AssetFile'ı medya dosyanızla ilgili bilgilerle güncelleştirmek için **BIRLEŞTIRME** HTTP isteğini kullanırsınız (konunun ilerleyen saatlerinde gösterildiği gibi).

**HTTP İsteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP Yanıt**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Yazma izniyle AccessPolicy oluşturma
Herhangi bir dosyayı blob depolamaalanına yüklemeden önce, bir varlığa yazmak için erişim ilkesi haklarını ayarlayın. Bunu yapmak için AccessPolicies varlık kümesine bir HTTP isteği gönderin. Oluşturulduktan sonra Bir DurationInMinutes değeri tanımlayın veya yanıt olarak 500 Dahili Sunucu hata iletisi alırsınız. AccessPolicys hakkında daha fazla bilgi için [AccessPolicy'ye](https://docs.microsoft.com/rest/api/media/operations/accesspolicy)bakın.

Aşağıdaki örnekte AccessPolicy nasıl oluşturulutur gösterilmektedir:

**HTTP İsteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"}

**HTTP Yanıt**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-the-upload-url"></a>Yükleme URL'sini alın

Gerçek yükleme URL'sini almak için bir SAS Bulucu oluşturun. Konumlayıcılar, Bir Varlıktaki Dosyalar'a erişmek isteyen istemciler için başlangıç saatini ve bağlantı bitiş noktasını tanımlar. Farklı istemci istek ve gereksinimlerini işlemek için belirli bir AccessPolicy ve Varlık çifti için birden çok Konumlayıcı varlık oluşturabilirsiniz. Bu Konum belirleyicilerin her biri, bir URL'nin kullanabileceği süreyi belirlemek için Başlangıç Zamanı değerini ve AccessPolicy'nin DurationInMinutes değerini kullanır. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/media/operations/locator)

Bir SAS URL'si aşağıdaki biçime sahiptir:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Bazı dikkate alınması gereken noktalar vardır:

* Belirli bir Varlıkla aynı anda ilişkili beşten fazla benzersiz Yer Bulucu'nuz olamaz. 
* Dosyalarınızı hemen yüklemeniz gerekiyorsa, Başlangıç Zamanı değerinizi geçerli saatin beş dakika öncesine ayarlamanız gerekir. Bunun nedeni, istemci makineniz ile Medya Hizmetleri arasında saat çarpıtması olabilir. Ayrıca, Başlangıç Saati değeriniz aşağıdaki DateTime biçiminde olmalıdır: YYYY-MM-DDTHH:mm:ssZ (örneğin, "2014-05-23T17:53:50Z").    
* Bir Locator oluşturulduktan sonra kullanıma hazır olduğunda 30-40 saniyelik bir gecikme olabilir. Bu sorun hem [SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) URL'si hem de Origin Locators için geçerlidir.

Aşağıdaki örnek, istek gövdesindeki Type özelliğinde tanımlandığı gibi bir SAS URL Bulucu'nun nasıl oluşturulurdu ("1" bir SAS bulucu için ve Isteğe Bağlı kaynak bulucu için "2" olarak) gösterir. Döndürülen **Yol** özelliği, dosyanızı yüklemek için kullanmanız gereken URL'yi içerir.

**HTTP İsteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178

    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP Yanıt**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Dosyayı blob depolama kabına yükleme
AccessPolicy ve Locator kümesine sahip olduktan sonra, gerçek dosya Azure Depolama REST API'leri kullanılarak azure blob depolama kapsayıcısına yüklenir. Dosyaları blok blobs olarak yüklemeniz gerekir. Sayfa lekeleri Azure Medya Hizmetleri tarafından desteklenmez.  

> [!NOTE]
> Yüklemek istediğiniz dosyanın dosya adını önceki bölümde alınan Konumbelirleme **Yolu** değerine eklemeniz gerekir. Örneğin, `https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?`.
>
>

Azure depolama lekeleri ile çalışma hakkında daha fazla bilgi için [Blob Service REST API'ye](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API)bakın.

### <a name="update-the-assetfile"></a>Varlık Dosyasını Güncelleştir
Dosyanızı yüklediğinize göre FileAsset boyutunu (ve diğer) bilgileri güncelleyin. Örnek:

    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: wamsbayclus001rest-hs.cloudapp.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP Yanıt**

Başarılı olursa, aşağıdaki döndürülür:

    HTTP/1.1 204 No Content
    ...

## <a name="delete-the-locator-and-accesspolicy"></a>Konum Bulucu ve AccessPolicy'yi silme
**HTTP İsteği**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP Yanıt**

Başarılı olursa, aşağıdaki döndürülür:

    HTTP/1.1 204 No Content
    ...

**HTTP İsteği**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP Yanıt**

Başarılı olursa, aşağıdaki döndürülür:

    HTTP/1.1 204 No Content
    ...

## <a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a><a id="encode"></a>Kaynak dosyayı uyarlanabilir bitrate MP4 dosyalarına kodlama

Varlıklar Medya Hizmetlerine alındıktan sonra, ortam istemcilere teslim edilmeden önce kodlanabilir, transmuxed, filigranlı ve benzeri olabilir. Bu etkinlikler, yüksek performans ve kullanılabilirlik sağlamak için birden fazla arka plan rol örneğinde zamanlanır ve çalıştırılır. Bu etkinliklere İşler denir ve her İş, Varlık dosyasındaki fiili çalışmayı yapan atomik Görevlerden oluşur (daha fazla bilgi için [Bkz. İş](https://docs.microsoft.com/rest/api/media/operations/job), [Görev](https://docs.microsoft.com/rest/api/media/operations/task) açıklamaları).

Daha önce de belirtildiği gibi, Azure Medya Hizmetleri ile çalışırken en yaygın senaryolardan biri, müşterilerinize uyarlanabilir bit hızı akışı sağlamaktır. Medya Hizmetleri, bir dizi uyarlanabilir bitrate MP4 dosyasını aşağıdaki biçimlerden birine dinamik olarak paketleyebilir: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.

Aşağıdaki bölümde, bir kodlama görevi içeren bir iş nasıl oluşturulacağını gösterir. Görev, asma dosyayı **Media Encoder Standard'ı**kullanarak uyarlanabilir bitrate MP4'lere dönüştürmek için belirtir. Bölüm ayrıca iş işleme ilerlemesinin nasıl izlendiğini de gösterir. İş tamamlandığında, varlıklarınıza erişmek için gereken yer bulucular oluşturabilirsiniz.

### <a name="get-a-media-processor"></a>Ortam işlemcisi alın
Medya Hizmetleri'nde ortam işlemcisi, medya içeriğini kodlama, biçim dönüştürme, şifreleme veya şifre çözme gibi belirli bir işleme görevini işleyen bir bileşendir. Bu öğreticide gösterilen kodlama görevi için Media Encoder Standard'ı kullanacağız.

Aşağıdaki kod kodlayıcının kimliğini ister.

**HTTP İsteği**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP Yanıt**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Bir iş oluşturma
Her İş, gerçekleştirmek istediğiniz işlem türüne bağlı olarak bir veya daha fazla Göreve sahip olabilir. REST API aracılığıyla, İşler'i ve ilgili Görevleri iki şekilde oluşturabilirsiniz: Görevler, İş varlıklarındaki Görevler gezinti özelliği veya OData toplu işlemi yoluyla satır satır olarak tanımlanabilir. Medya Hizmetleri SDK toplu işleme kullanır. Ancak, bu makaledeki kod örneklerinin okunabilirliği için görevler satır satır olarak tanımlanır. Toplu işleme hakkında bilgi için [Açık Veri Protokolü (OData) Toplu İşleme'ye](https://www.odata.org/documentation/odata-version-3-0/batch-processing/)bakın.

Aşağıdaki örnek, belirli bir çözünürlükte ve kalitede bir video kodlamak için tek bir Görev kümesiyle bir İş'i nasıl oluşturup gönderebildiğinizi gösterir. Aşağıdaki dokümantasyon bölümü, Media Encoder Standart işlemci tarafından desteklenen tüm [görev hazır ayarlarının](https://msdn.microsoft.com/library/mt269960) listesini içerir.  

**HTTP İsteği**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"Adaptive Streaming",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP Yanıt**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  

             ]
          }
       }
    }


Herhangi bir İş isteğinde dikkat edilmesi gereken birkaç önemli nokta vardır:

* TaskBody özellikleri, Görev tarafından kullanılan giriş veya çıktı varlıklarını tanımlamak için gerçek XML kullanmalı. Görev makalesi XML için XML Şema Tanımı içerir.
* TaskBody tanımında, her iç `<inputAsset>` `<outputAsset>` değer için ve JobInputAsset(değer) veya JobOutputAsset (değer) olarak ayarlanmalıdır.
* Bir görevin birden çok çıktı varlığı olabilir. Bir JobOutputAsset(x) bir işte bir görevin çıktısı olarak yalnızca bir kez kullanılabilir.
* JobInputAsset veya JobOutputAsset'i görevin giriş varlığı olarak belirtebilirsiniz.
* Görevler bir döngü oluşturmamalıdır.
* JobInputAsset veya JobOutputAsset'e geçtiğiniz değer parametresi, bir Varlığın dizin değerini temsil eder. Gerçek Varlıklar, İş varlığı tanımındaki InputMediaAssets ve OutputMediaAssets navigasyon özelliklerinde tanımlanır.

> [!NOTE]
> Medya Hizmetleri OData v3 üzerine kurulduğundan, InputMediaAssets ve OutputMediaAssets navigasyon özellik koleksiyonlarındaki tek tek varlıklar "__metadata : uri" ad değeri çifti aracılığıyla başvurulur.
>
>

* InputMediaAssets, Medya Hizmetleri'nde oluşturduğunuz bir veya daha fazla Varlıkla eşler. OutputMediaAssets sistem tarafından oluşturulur. Varolan bir varlığa başvurmaz.
* OutputMediaAssets assetName özniteliği kullanılarak adlandırılabilir. Bu öznitelik yoksa, OutputMediaAsset'in adı, `<outputAsset>` öğenin iç metin değerinin, İş Adı değerinin veya İş Kimliği değerinin soneğiyle (Ad özelliğinin tanımlanmamış olması durumunda) ne olursa olsun. Örneğin, varlık Adı için bir değer "Örnek" olarak ayarlarsanız, OutputMediaAsset Adı özelliği "Örnek" olarak ayarlanır. Ancak, assetName için bir değer belirlemediyseniz, ancak iş adını "NewJob" olarak ayarladıysanız, OutputMediaAsset Adı "JobOutputAsset(değer)_NewJob" olacaktır.

    Aşağıdaki örnek, varlık Adı özniteliğinin nasıl ayarlanır:

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* Görev zincirleme etkinleştirmek için:

  * Bir işin en az iki görevi olmalıdır
  * Girdisi işteki başka bir görevin çıktısı olan en az bir görev olmalıdır.

Daha fazla bilgi için bkz: [Media Services REST API ile Kodlama İşi Oluşturma.](media-services-rest-encode-asset.md)

### <a name="monitor-processing-progress"></a>İşlem İlerlemeyi İzle
Aşağıdaki örnekte gösterildiği gibi, Devlet özelliğini kullanarak İş durumunu alabilirsiniz:

**HTTP İsteği**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP Yanıt**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT

    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Bir işi iptal etme
Medya Hizmetleri, CancelJob işlevi aracılığıyla çalışan işleri iptal etmenizi sağlar. Bu arama, durumu iptal edildiğinde, iptal edildiğinde, iptal edildiğinde, hata yaptığında veya tamamlandığında Bir İş'i iptal etmeye çalışırsanız 400 hata kodu döndürür.

Aşağıdaki örnek, CancelJob'un nasıl adlandırılabildiğini gösterir.

**HTTP İsteği**

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net


Başarılı olursa, 204 yanıt kodu ileti gövdesi olmadan döndürülür.

> [!NOTE]
> CancelJob'a parametre olarak aktarırken iş kimliğini (normalde nb:jid:UUID: somevalue) URL kodlamanız gerekir.
>
>

### <a name="get-the-output-asset"></a>Çıktı varlığını alma
Aşağıdaki kod, çıktı varlık Kimliği'nin nasıl istenecek lerini gösterir.

**HTTP İsteği**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP Yanıt**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }

## <a name="publish-the-asset-and-get-streaming-and-progressive-download-urls-with-rest-api"></a><a id="publish_get_urls"></a>Varlığı yayımlayın ve REST API ile akış ve aşamalı indirme URL'leri alın

Bir varlığı akışla aktarmak veya indirmek için söz konusu varlığı önce bir bulucu oluşturarak “yayımlamak” gerekir. Bulucular varlıkta bulunan dosyalara erişim imkanı sağlar. Media Services, iki tür bulucuyu destekler: Medyayı akışla aktarmak (örneğin MPEG DASH, HLS veya Kesintisiz Akış) için kullanılan OnDemandOrigin bulucuları ve medya dosyalarını indirmek için kullanılan Erişim İmzası (SAS) bulucuları. 

Yer bulucuları oluşturduktan sonra, dosyalarınızı akışveya indirmek için kullanılan URL'leri oluşturabilirsiniz.

>[!NOTE]
>AMS hesabınız **oluşturulduğunda,** **Durduruldu** durumunda hesabınıza varsayılan akış bitiş noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir.

Kesintisiz Akışa ilişkin bir akış URL'si aşağıdaki biçime sahiptir:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS’ye ilişkin bir akış URL'si aşağıdaki biçime sahiptir:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH’e ilişkin bir akış URL'si aşağıdaki biçime sahiptir:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Dosya indirmek için kullanılan bir SAS URL'si aşağıdaki biçime sahiptir:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Bu bölümde, varlıklarınızı "yayınlamak" için gereken aşağıdaki görevlerin nasıl gerçekleştirililecektir gösterilmektedir.  

* Okuma izniyle AccessPolicy oluşturma
* İçerik indirmek için SAS URL'si oluşturma
* İçerik akışı için başlangıç URL'si oluşturma

### <a name="creating-the-accesspolicy-with-read-permission"></a>Okuma izniyle AccessPolicy oluşturma
Herhangi bir medya içeriğini indirmeden veya yayınlamadan önce, önce okuma izinleriyle bir AccessPolicy tanımlayın ve müşterileriniz için etkinleştirmek istediğiniz teslim mekanizmasının türünü belirten uygun Konumbelirleyici varlık oluşturun. Kullanılabilir özellikler hakkında daha fazla bilgi için [AccessPolicy Entity Properties'e](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties)bakın.

Aşağıdaki örnekte, belirli bir Varlık için okuma izinleri için Access Policy'nin nasıl belirtileceğini gösterilmektedir.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Başarılı olursa, oluşturduğunuz AccessPolicy kuruluşunu açıklayan bir 201 başarı kodu döndürülür. Daha sonra AccessPolicy Id'yi, Teslim etmek istediğiniz dosyayı (çıktı varlığı gibi) içeren varlığın Varlık Kimliği ile birlikte Konumbelirleme varlığını oluşturmak için kullanırsınız.

> [!NOTE]
> Bu temel iş akışı, bir Varlığı sindirirken dosya yüklemekle aynıdır (bu konuda daha önce tartışıldığı gibi). Ayrıca, dosya yükleme gibi, dosyalarınıza (veya müşterilerinizin) hemen erişmeniz gerekiyorsa, Başlangıç Zamanı değerinizi geçerli zamandan beş dakika öncesine ayarlayın. İstemci ve Medya Hizmetleri arasında saat eğriliği olabileceğinden, bu eylem gereklidir. Başlangıç Saati değeri aşağıdaki DateTime biçiminde olmalıdır: YYYY-MM-DDTHH:mm:ssZ (örneğin, "2014-05-23T17:53:50Z").
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a>İçerik indirmek için SAS URL'si oluşturma
Aşağıdaki kod, daha önce oluşturulmuş ve yüklenen bir ortam dosyasını indirmek için kullanılabilecek bir URL'yi nasıl elde edebileceğinizi gösterir. AccessPolicy izinleri belirlemiştir ve Konum belirleme yolu Bir SAS indirme URL'sine başvurur.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }

Döndürülen **Yol** özelliği SAS URL'sini içerir.

> [!NOTE]
> Depolama şifrelenmiş içeriği karşıdan yükledikten önce, bu içeriği oluşturmadan önce el ile şifreçözme işlemini kullanmanız veya işlenmiş dosyaları bir OutputAsset'e açık olarak çıktılamak ve ardından bu Varlıktan indirmek için bir işleme görevinde Depolama Şifre Çözme MediaProcessor'i kullanmanız gerekir. İşleme hakkında daha fazla bilgi için bkz. Ayrıca, SAS URL Yer bulucuları oluşturulduktan sonra güncelleştirilemez. Örneğin, aynı Locator'u güncelleştirilmiş bir Başlangıç Zamanı değeriyle yeniden kullanamazsınız. Bunun nedeni, SAS URL'lerinin oluşturulma biçimidir. Bir Konum belirleyicinin süresi dolduktan sonra indirilebilmek için bir varlığa erişmek istiyorsanız, yeni bir Başlangıç Saati ile yeni bir kıymet oluşturmanız gerekir.
>
>

### <a name="download-files"></a>Dosyaları indirme
AccessPolicy ve Locator kümesini ayarladıktan sonra, Azure Depolama REST API'lerini kullanarak dosyaları indirebilirsiniz.  

> [!NOTE]
> İndirmek istediğiniz dosyanın dosya adını önceki bölümde alınan Konumbelirleme **Yolu** değerine eklemeniz gerekir. Örneğin, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . .

Azure depolama lekeleri ile çalışma hakkında daha fazla bilgi için [Blob Service REST API'ye](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API)bakın.

Daha önce gerçekleştirdiğiniz kodlama işinin (Uyarlamalı MP4 kümesine kodlama) bir sonucu olarak, aşamalı olarak indirebileceğiniz birden çok MP4 dosyanız var. Örnek:    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

### <a name="creating-a-streaming-url-for-streaming-content"></a>İçerik akışı için akış URL'si oluşturma
Aşağıdaki kod, akışlı URL Bulucu'nun nasıl oluşturulabildiğini gösterir:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Akışlı bir ortam oynatıcıda Düzgün Akış kaynağı URL'si akışı sağlamak için, Yol özelliğini Düzgün Akış bildirimi dosyasının adıyla ve ardından "/manifest" ile iliştirmeniz gerekir.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS akışı için "/manifest"den sonra (format=m3u8-aapl) ekin.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH'i akış için "/manifest"den sonra (format=mpd-time-csf) ekine.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a name="play-your-content"></a><a id="play"></a>İçeriğinizi oynatma
Videonuzu akışla aktarmak için [Azure Media Services Oynatıcısı](https://aka.ms/azuremediaplayer)’nı kullanın.

Aşamalı karşıdan yüklemeyi test etmek için bir URL'yi tarayıcıya yapıştırın (örneğin, IE, Chrome, Safari).

## <a name="next-steps-media-services-learning-paths"></a>Sonraki Adımlar: Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
