---
title: REST kullanarak dosyaları Azure Medya Hizmetleri hesabına yükleme | Microsoft Dokümanlar
description: Varlıklar oluşturup yükleyerek medya içeriğini Medya Hizmetleri'ne nasıl dahil ediniz öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: d5b84a9d216457720e9bd4e17b002d6ab9490f9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73888593"
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>REST kullanarak Media Services hesabına dosya yükleme  
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [Geri kalanı](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 

Media Services’de dijital dosyalar bir varlığa yüklenir. [Varlık](https://docs.microsoft.com/rest/api/media/operations/asset) varlığı video, ses, görüntü, küçük resim koleksiyonları, metin parçaları ve kapalı altyazı dosyaları (ve bu dosyalarla ilgili meta veriler) içerebilir.  Dosyalar varlığa yüklendikten sonra, içeriğiniz daha fazla işlem ve akış için bulutta güvenli bir şekilde saklanır. 

Bu öğreticide, bir dosyayı ve bununla ilişkili diğer işlemleri nasıl yükleyersiniz:

> [!div class="checklist"]
> * Tüm yükleme işlemleri için Postacı'yı ayarlama
> * Media Services’e bağlanmak 
> * Yazma izniyle bir erişim ilkesi oluşturma
> * Varlık oluşturma
> * Bir SAS bulucu oluşturun ve yükleme URL'sini oluşturun
> * Yükleme URL'sini kullanarak blob depolama alanına dosya yükleme
> * Yüklediğiniz medya dosyası için kıymette bir meta veri oluşturma

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bir hesap oluşturun.
- [Azure portalını kullanarak bir Azure Medya Hizmetleri hesabı oluşturun.](media-services-portal-create-account.md)
- [AAD kimlik doğrulama genel bakış makalesiile Erişim Azure Medya Hizmetleri API'sini gözden geçirin.](media-services-use-aad-auth-to-access-ams-api.md)
- Ayrıca daha fazla bilgi için REST makalesi [ile Medya Hizmetleri API'sine erişmek için Azure AD kimlik doğrulamasını kullanın'ı gözden geçirin.](https://docs.microsoft.com/azure/media-services/previous/media-services-rest-connect-with-aad)
- Medya [Hizmetleri REST API çağrıları için Yapılandırma Postacı](media-rest-apis-with-postman.md)açıklandığı gibi **Postacı** yapılandırın.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Media Services REST API kullanırken aşağıdaki hususlar geçerlidir:
 
* Media Services REST API'yi kullanarak varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri ayarlamanız gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API Geliştirme kurulumu'na](media-services-rest-how-to-use.md)bakın. <br/>Bu öğreticide kullanılan Postacı koleksiyonu, gerekli tüm üstbilgilerin ayarını yapar.
* Medya Hizmetleri, akış içeriği için URL'ler inşa ederken IAssetFile.Name özelliğinin değerini kullanır (örneğin, http://{AMSAccount}.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Bu nedenle, yüzde kodlama izin verilmez. **Ad** özelliğinin değeri aşağıdaki [yüzde kodlama-ayrılmış karakterlerden](https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)hiçbirine sahip olamaz : !*'();:@&=+$,/?%#[]". Ayrıca, dosya adı uzantısı için yalnızca bir '.' olabilir.
* Adın uzunluğu 260 karakterden büyük olmamalıdır.
* Media Services ile işleme için desteklenen dosya boyutlarına yönelik üst sınır uygulanır. Dosya boyutu sınırlaması hakkında ayrıntılı bilgi için [bu](media-services-quotas-and-limitations.md) makaleye bakın.

## <a name="set-up-postman"></a>Postman’i ayarlama

Bu öğretici için Postacı'yı nasıl ayarlayabilmek için adımlar için [Postacı'yı Yapılandır'a](media-rest-apis-with-postman.md)bakın.

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

1. Ortamınıza bağlantı değerleri ekleyin. 

    **MediaServices** [ortamının](postman-environment.md) bir parçası olan bazı değişkenlerin, [koleksiyonda](postman-collection.md)tanımlanan işlemleri yürütmeye başlayabilmeniz için el ile ayarlanması gerekir.

    İlk beş değişkenin değerlerini almak için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-import-env.png)
2. **MediaFileName** ortamı değişkeninin değerini belirtin.

    Yüklemeyi planladığınız ortamın dosya adını belirtin. Bu örnekte, BigBuckBunny.mp4 yüklemek için gidiyoruz. 
3. **AzureMediaServices.postman_environment.json** dosyasını inceleyin. Koleksiyondaki hemen hemen tüm işlemlerin bir "test" komut dosyası yürüttüğünün göreceksiniz. Komut dosyaları yanıt tarafından döndürülen bazı değerleri alır ve uygun ortam değişkenleri ayarlar.

    Örneğin, ilk işlem bir erişim belirteci alır ve diğer tüm işlemlerde kullanılan **AccessToken** ortamı değişkenine ayarlar.

    ```    
    "listen": "test",
    "script": {
        "type": "text/javascript",
        "exec": [
            "var json = JSON.parse(responseBody);",
            "postman.setEnvironmentVariable(\"AccessToken\", json.access_token);"
        ]
    }
    ```
4. **Postacı** penceresinin solunda **1'e tıklayın. AAD Auth belirteci** -> **Alın Hizmet Sorumlusu için Azure AD Belirteci alın.**

    URL bölümü **AzureADSTSEndpoint** ortamı değişkeni ile doldurulur (öğreticinin başlarında, koleksiyonu destekleyen ortam değişkenlerinin değerlerini ayarlarsınız).

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postment-get-token.png)

5. **Gönder**’e basın.

    "access_token" içeren yanıtı görebilirsiniz. "Test" komut dosyası bu değeri alır ve **AccessToken** ortamı değişkenini ayarlar (yukarıda açıklandığı gibi). Ortam değişkenlerinizi incelerseniz, bu değişkenin artık işlemlerin geri kalanında kullanılan erişim belirteci (taşıyıcı belirteci) değerini içerdiğini görürsünüz. 

    Belirteç süresi dolduğunda, yeniden "Hizmet Sorumlusu için Azure AD Belirteci Alın" adımından geçin. 

## <a name="create-an-access-policy-with-write-permission"></a>Yazma izniyle bir erişim ilkesi oluşturma

### <a name="overview"></a>Genel Bakış 

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

Herhangi bir dosyayı blob depolamaalanına yüklemeden önce, bir varlığa yazmak için erişim ilkesi haklarını ayarlayın. Bunu yapmak için AccessPolicies varlık kümesine bir HTTP isteği gönderin. Oluşturulduktan sonra Bir DurationInMinutes değeri tanımlayın veya yanıt olarak 500 Dahili Sunucu hata iletisi alırsınız. AccessPolicys hakkında daha fazla bilgi için [AccessPolicy'ye](https://docs.microsoft.com/rest/api/media/operations/accesspolicy)bakın.

### <a name="create-an-access-policy"></a>Erişim ilkesi oluşturma

1. Upload için **AccessPolicy** -> **Create AccessPolicy'yi**seçin.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-access-policy.png)

    "Test" komut dosyası AccessPolicy Id alır ve uygun ortam değişkeni ayarlar.

## <a name="create-an-asset"></a>Varlık oluşturma

### <a name="overview"></a>Genel Bakış

[Varlık,](https://docs.microsoft.com/rest/api/media/operations/asset) Video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı altyazı dosyaları da dahil olmak üzere Medya Hizmetleri'ndeki birden çok nesne türü veya kümeiçin bir kapsayıcıdır. REST API'de, bir Varlık oluşturmak için Medya Hizmetlerine POSTA isteği gönderilmesi ve kıymetinizle ilgili tüm özellik bilgilerini istek gövdesine yerleştirmeniz gerekmektedir.

Bir varlık oluştururken ekleyebileceğiniz özelliklerden **Options**biri Seçenekler'dir. Aşağıdaki şifreleme seçeneklerinden birini belirtebilirsiniz: **Yok** (varsayılan, şifreleme kullanılmaz), **StorageEncrypted** (istemci tarafı depolama şifrelemesi ile önceden şifrelenmiş içerik için), **CommonEncryptionProtected**veya **EnvelopeEncryptionProtected**. Şifreli bir kıymetinz olduğunda, bir teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için [bkz.](media-services-rest-configure-asset-delivery-policy.md)

Varlığınız şifrelenmişse, bir **ContentKey** oluşturmalı ve aşağıdaki makalede açıklandığı gibi kıymetinize bağlamanız gerekir: [ContentKey nasıl oluşturulur.](media-services-rest-create-contentkey.md) Dosyaları varlığa yükledikten sonra, **Varlık Dosyası** varlığındaki şifreleme özelliklerini **Varlık** şifrelemesi sırasında sahip olduğunuz değerlerle güncelleştirmeniz gerekir. **BIRLEŞTIRME** HTTP isteğini kullanarak yapın. 

Bu örnekte, şifrelenmemiş bir varlık oluşturuyoruz. 

### <a name="create-an-asset"></a>Varlık oluşturma

1. **Varlık** -> **Oluştur'u**seçin.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-asset.png)

    "Test" komut dosyası Varlık Kimliğini alır ve uygun ortam değişkenini ayarlar.

## <a name="create-a-sas-locator-and-create-the-upload-url"></a>Bir SAS bulucu oluşturun ve Yükleme URL'sini oluşturun

### <a name="overview"></a>Genel Bakış

AccessPolicy ve Locator kümesine sahip olduktan sonra, gerçek dosya Azure Depolama REST API'leri kullanılarak azure blob depolama kapsayıcısına yüklenir. Dosyaları blok blobs olarak yüklemeniz gerekir. Sayfa lekeleri Azure Medya Hizmetleri tarafından desteklenmez.  

Azure depolama lekeleri ile çalışma hakkında daha fazla bilgi için [Blob Service REST API'ye](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API)bakın.

Gerçek yükleme URL'sini almak için bir SAS Bulucu oluşturun (aşağıda gösterilmiştir). Konumlayıcılar, Bir Varlıktaki Dosyalar'a erişmek isteyen istemciler için başlangıç saatini ve bağlantı bitiş noktasını tanımlar. Farklı istemci istek ve gereksinimlerini işlemek için belirli bir AccessPolicy ve Varlık çifti için birden çok Konumlayıcı varlık oluşturabilirsiniz. Bu Konum belirleyicilerin her biri, bir URL'nin kullanabileceği süreyi belirlemek için Başlangıç Zamanı değerini ve AccessPolicy'nin DurationInMinutes değerini kullanır. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/media/operations/locator)

Bir SAS URL'si aşağıdaki biçime sahiptir:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Bazı dikkate alınması gereken noktalar vardır:

* Belirli bir Varlıkla aynı anda ilişkili beşten fazla benzersiz Yer Bulucu'nuz olamaz. Daha fazla bilgi için bkz.
* Dosyalarınızı hemen yüklemeniz gerekiyorsa, Başlangıç Zamanı değerinizi geçerli saatin beş dakika öncesine ayarlamanız gerekir. Bunun nedeni, istemci makineniz ile Medya Hizmetleri arasında saat çarpıtması olabilir. Ayrıca, Başlangıç Saati değeriniz aşağıdaki DateTime biçiminde olmalıdır: YYYY-MM-DDTHH:mm:ssZ (örneğin, "2014-05-23T17:53:50Z").    
* Bir Locator oluşturulduktan sonra kullanıma hazır olduğunda 30-40 saniyelik bir gecikme olabilir.

### <a name="create-a-sas-locator"></a>SAS bulucu oluşturma

1. **SAS** -> **Bulucu Oluştur'u**seçin.
2. **Gönder**’e basın.

    "Test" komut dosyası, belirttiğiniz ortam dosyası adını ve SAS yer bulucu bilgilerini temel alan "Yükleme URL'si" oluşturur ve uygun ortam değişkenini ayarlar.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-sas-locator.png)

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Yükleme URL'sini kullanarak blob depolama alanına dosya yükleme

### <a name="overview"></a>Genel Bakış

Yükleme URL'sine sahip olduğunuza göre, dosyanızı Doğrudan SAS kapsayıcısına yüklemek için Azure Blob API'lerini kullanarak bazı kodlar yazmanız gerekir. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Depolama REST API’sini kullanma](https://docs.microsoft.com/azure/storage/common/storage-rest-api-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [BLOB KOY](https://docs.microsoft.com/rest/api/storageservices/put-blob)
- [Blob depolama alanına blobs yükleyin](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#upload-blobs-to-blob-storage)

### <a name="upload-a-file-with-postman"></a>Postacı ile dosya yükleme

Örnek olarak, küçük bir .mp4 dosyası yüklemek için Postacı'yı kullanırız. Postman aracılığıyla ikili yükleme bir dosya boyutu sınırı olabilir.

Yükleme isteği **AzureMedia** koleksiyonunun bir parçası değildir. 

Yeni bir istek oluşturun ve ayarlayın:
1. Yeni **+** bir istek sekmesi oluşturmak için , basın.
2. **URL'de PUT** işlemini ve **{{UploadURL}}** yapıştır'ı seçin.
2. **Yetkilendirme** sekmesini olduğu gibi bırakın **(Taşıyıcı Belirteci'ne**ayarlamayın).
3. **Üstbilgi** sekmesinde şunu belirtin: **Anahtar**: "x-ms-blob-type" ve **Değer**: "BlockBlob".
2. **Gövde** sekmesinde **ikili'yi**tıklatın.
4. **MediaFileName** ortam değişkeninde belirttiğiniz adı içeren dosyayı seçin.
5. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-upload-file.png)

##  <a name="create-a-metadata-in-the-asset"></a>Kıymette bir meta veri oluşturma

Dosya yüklendikten sonra, kıymetinizle ilişkili blob depolamasına yüklediğiniz ortam dosyası için varlıkta bir meta veri oluşturmanız gerekir.

1. **AssetFiles** -> **CreateFileInfos'u**seçin.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-file-info.png)

Dosya yüklenmeli ve meta veri seti ayarlanmalıdır.

## <a name="validate"></a>Doğrulama

Dosyanın başarıyla yüklendiğini doğrulamak [için, AssetFile'ı](https://docs.microsoft.com/rest/api/media/operations/assetfile) sorgulamak ve **ContentFileSize'ı** (veya diğer ayrıntıları) yeni varlıkta görmeyi beklediğiniz ile karşılaştırmak isteyebilirsiniz. 

Örneğin, aşağıdaki **GET** işlemi varlık dosyanız (bigBuckBunny.mp4 dosyasında) dosya verilerini getirir. Sorgu, daha önce ayarladığınız [ortam değişkenlerini](postman-environment.md) kullanıyor.

    {{RESTAPIEndpoint}}/Assets('{{LastAssetId}}')/Files

Yanıt boyutu, adı ve diğer bilgileri içerir.

    "Id": "nb:cid:UUID:69e72ede-2886-4f2a-8d36-80a59da09913",
    "Name": "BigBuckBunny.mp4",
    "ContentFileSize": "3186542",
    "ParentAssetId": "nb:cid:UUID:0b8f3b04-72fb-4f38-8e7b-d7dd78888938",
            
## <a name="next-steps"></a>Sonraki adımlar

Karşıya yüklenen varlıklarınızı artık kodlayabilirsiniz. Daha fazla bilgi için bkz. [Varlıkları kodlama](media-services-portal-encode.md).

Yapılandırılmış kapsayıcıya gelen dosyaya göre bir kodlama işi tetiklemek için Azure İşlevleri’ni de kullanabilirsiniz. Daha fazla bilgi için [bu örneğe](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) bakın.

