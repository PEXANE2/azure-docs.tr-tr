---
title: v3 API'leri ile geliştirin
titleSuffix: Azure Media Services
description: Media Services v3 ile geliştirirken varlıklar ve API'ler için geçerli olan kurallar hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/21/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: eacdfe8211c97e75b6609f5e11b681f84ae55846
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472093"
---
# <a name="develop-with-media-services-v3-apis"></a>Medya Hizmetleri v3 API'leri ile geliştirin

Geliştirici olarak, özel medya iş akışlarını kolayca oluşturmak, yönetmek ve korumak için REST API ile etkileşimkurmanıza olanak tanıyan Media Services [REST API](https://docs.microsoft.com/rest/api/media/) veya istemci kitaplıklarını kullanabilirsiniz. [Medya Hizmetleri v3](https://aka.ms/ams-v3-rest-sdk) API OpenAPI belirtimi (eski bir Swagger olarak bilinir) dayanmaktadır.

Bu makalede, Media Services v3 ile geliştirdiğiniz zaman varlıklar ve API'ler için geçerli olan kurallar açıklanır.

## <a name="accessing-the-azure-media-services-api"></a>Azure Medya Hizmetleri API'sine erişim

Medya Hizmetleri kaynaklarına ve Medya Hizmetleri API'sine erişme yetkisine ulaşmak için öncelikle kimlik doğrulaması yapılması gerekir. Medya [Hizmetleri, Azure Etkin Dizin (Azure AD) tabanlı](../../active-directory/fundamentals/active-directory-whatis.md) kimlik doğrulamasını destekler. İki yaygın kimlik doğrulama seçeneği şunlardır:
 
* **Hizmet temel kimlik doğrulaması**: Bir hizmeti doğrulamak için kullanılır (örneğin: web uygulamaları, işlev uygulamaları, mantık uygulamaları, API ve mikro hizmetler). Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, daemon hizmetlerini, orta katman hizmetlerini veya zamanlanmış işleri çalıştıran uygulamalardır. Örneğin, web uygulamaları için her zaman Bir Hizmet Yöneticisi ile Medya Hizmetleri bağlanan bir orta katman olmalıdır.
* **Kullanıcı kimlik doğrulaması**: Medya Hizmetleri kaynaklarıyla etkileşimde kalmak için uygulamayı kullanan bir kişinin kimliğini doğrulamak için kullanılır. Etkileşimli uygulama öncelikle kullanıcının kimlik bilgilerini ister. Bir örnek, kodlama işlerini veya canlı akışı izlemek için yetkili kullanıcılar tarafından kullanılan bir yönetim konsolu uygulamasıdır.

Medya Hizmetleri API'si, REST API isteklerini yapan kullanıcının veya uygulamanın Medya Hizmetleri hesap kaynağına erişebulaşmasını ve **Katılımcı** veya **Sahip** rolü kullanmasını gerektirir. API'ye **Reader** rolüyle erişilebilir, ancak yalnızca **Get** or **List** işlemleri kullanılabilir.Daha fazla bilgi için Medya [Hizmetleri hesapları için Rol tabanlı erişim denetimine](rbac-overview.md)bakın.

Bir hizmet ilkesi oluşturmak yerine, Azure Kaynak Yöneticisi aracılığıyla Medya Hizmetleri API'sine erişmek için Azure kaynakları için yönetilen kimlikler kullanmayı düşünün. Azure kaynakları için yönetilen kimlikler hakkında daha fazla bilgi edinmek için Azure [kaynakları için yönetilen kimliklerin ne olduğunu](../../active-directory/managed-identities-azure-resources/overview.md)öğrenin.

### <a name="azure-ad-service-principal"></a>Azure AD hizmet ilkesi

Bir Azure AD uygulaması ve hizmet ilkesi oluşturuyorsanız, uygulamanın kendi kiracısında olması gerekir. Uygulamayı oluşturduktan sonra, **uygulamanın Medya** Hizmetleri hesabına Katkıda Bulunan veya **Sahip** rolüne erişim izni verin.

Bir Azure AD uygulaması oluşturma izniniz olup olmadığından emin değilseniz, [gerekli izinlere](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)bakın.

Aşağıdaki şekilde, sayılar isteklerin akışını kronolojik sırada temsil emretmek:

![Bir web API'sinden AAD ile orta katman uygulama kimlik doğrulaması](./media/use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

1. Orta katmandaki bir uygulama, aşağıdaki parametrelere sahip bir Azure AD erişim belirteci ister:  

   * Azure AD kiracı bitiş noktası.
   * Medya Hizmetleri kaynak URI.
   * REST Medya Hizmetleri için Kaynak URI.
   * Azure AD uygulama değerleri: istemci kimliği ve istemci sırrı.

   Gerekli tüm değerleri elde etmek için [Azure CLI ile Azure Medya Hizmetleri API'sine eriş'e](access-api-cli-how-to.md)bakın.

2. Azure AD erişim belirteci orta katmana gönderilir.
4. Orta katman, Azure AD belirteciyle birlikte Azure Media REST API'sine istek gönderir.
5. Orta katman, Medya Hizmetleri'nden gelen verileri geri alır.

### <a name="samples"></a>Örnekler

Azure AD hizmet ilkesiyle nasıl bağlanılabildiğini gösteren aşağıdaki örneklere bakın:

* [REST ile bağlanın](media-rest-apis-with-postman.md)  
* [Java ile bağlanma](configure-connect-java-howto.md)
* [.NET ile bağlanma](configure-connect-dotnet-howto.md)
* [Node.js ile bağlanma](configure-connect-nodejs-howto.md)
* [Python ile bağlanma](configure-connect-python-howto.md)

## <a name="naming-conventions"></a>Adlandırma kuralları

Azure Media Services v3 kaynaklarının adları (Varlıklar, İşler, Dönüşümler gibi), Azure Resource Manager adlandırma kısıtlamalarına tabidir. Azure Resource Manager uyarınca kaynak adları her zaman benzersizdir. Bu nedenle kaynaklarınızda benzersiz tanıtıcı dizeleri (GUID gibi) kullanabilirsiniz.

Medya Hizmetleri kaynak adları şunları içeremez: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.,,..,,,',tek tırnak karakteri veya herhangi bir denetim karakteri. Diğer tüm karakterlere izin verilir. Bir kaynağın adı en fazla 260 karakter olabilir.

Azure Kaynak Yöneticisi adlandırma hakkında daha fazla [Naming conventions](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)bilgi için [bkz.](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource)

### <a name="names-of-filesblobs-within-an-asset"></a>Bir varlık içindeki dosyaların/blobların adları

Bir varlık içindeki dosyaların/blobs adlarının hem [blob adı gereksinimlerini](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) hem de [NTFS ad gereksinimlerini](https://docs.microsoft.com/windows/win32/fileio/naming-a-file)izlemesi gerekir. Bu gereksinimlerin nedeni, dosyaların blob depolamadan yerel bir NTFS diskine kopyalanabiliyor olmasıdır.

## <a name="long-running-operations"></a>Uzun süren operasyonlar

Azure Medya `x-ms-long-running-operation` Hizmetleri [dosyalarında](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/streamingservice.json) işaretlenen işlemler uzun süren işlemlerdir. 

Eşzamanlı Azure işlemlerinin nasıl izlenir hakkında ayrıntılı bilgi için [Async işlemlerine](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations#monitor-status-of-operation)bakın.

Medya Hizmetleri'nin aşağıdaki uzun soluklu işlemleri vardır:

* [Canlı Etkinlikler Oluşturun](https://docs.microsoft.com/rest/api/media/liveevents/create)
* [Canlı Etkinlikleri Güncelleştir](https://docs.microsoft.com/rest/api/media/liveevents/update)
* [Canlı Etkinliği Silme](https://docs.microsoft.com/rest/api/media/liveevents/delete)
* [Canlı Etkinliği Başlat](https://docs.microsoft.com/rest/api/media/liveevents/start)
* [LiveEvent'i Durdur](https://docs.microsoft.com/rest/api/media/liveevents/stop)

  Olayı `removeOutputsOnStop` durdururken ilişkili tüm Canlı Çıktıları silmek için parametreyi kullanın.  
* [LiveEvent'i Sıfırla](https://docs.microsoft.com/rest/api/media/liveevents/reset)
* [LiveOutput oluşturma](https://docs.microsoft.com/rest/api/media/liveevents/create)
* [LiveOutput silme](https://docs.microsoft.com/rest/api/media/liveevents/delete)
* [StreamingEndpoint oluşturma](https://docs.microsoft.com/rest/api/media/streamingendpoints/create)
* [StreamingEndpoint'i güncelleştir](https://docs.microsoft.com/rest/api/media/streamingendpoints/update)
* [StreamingEndpoint silme](https://docs.microsoft.com/rest/api/media/streamingendpoints/delete)
* [StreamingEndpoint'i başlat](https://docs.microsoft.com/rest/api/media/streamingendpoints/start)
* [StreamingEndpoint'i Durdur](https://docs.microsoft.com/rest/api/media/streamingendpoints/stop)
* [StreamingEndpoint'i Ölçeklendir](https://docs.microsoft.com/rest/api/media/streamingendpoints/scale)

Uzun bir işlemin başarılı bir şekilde gönderilmesi üzerine bir '202 Kabul Edildi' alırsınız ve döndürülen işlem kimliğini kullanarak işlemin tamamlanması için anket yapmalısınız.

[Parça eşzamanlı Azure işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) makalesi, yanıtta döndürülen değerler aracılığıyla eşzamanlı Azure işlemlerinin durumunu nasıl izleyebilirsiniz ayrıntılı olarak açıklar.

Belirli bir Canlı Etkinlik veya ilişkili Canlı Çıktılarından herhangi biri için yalnızca bir uzun süren işlem desteklenir. Başlatıldıktan sonra, aynı LiveEvent'de veya ilişkili Canlı Çıktılar'da sonraki uzun süren bir işlemi başlatmadan önce uzun süren bir işlemin tamamlanması gerekir. Birden çok Canlı Çıkışa sahip Canlı Etkinlikler için, başka bir Canlı Çıktı üzerinde uzun süren bir işlemi tetiklemeden önce bir Canlı Çıktı üzerinde uzun süren bir işlemin tamamlanmasını beklemeniz gerekir. 

## <a name="sdks"></a>SDK’lar

> [!NOTE]
> Azure Medya Hizmetleri v3 SDK'larının iş parçacığı için güvenli olduğu garanti edilmez. Çok iş parçacığı uygulaması geliştirirken, istemciyi korumak veya iş parçacığı başına yeni bir AzureMediaServicesClient nesnesi kullanmak için kendi iş parçacığı eşitleme mantığınızı eklemeniz gerekir. Ayrıca, kodunuz tarafından istemciye sağlanan isteğe bağlı nesneler tarafından sunulan çoklu iş parçacığı sorunlarına da dikkat etmelisiniz (.NET'teki bir HttpClient örneği gibi).

|SDK|Başvuru|
|---|---|
|[.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)|[.NET başvurusu](https://aka.ms/ams-v3-dotnet-ref)|
|[Java SDK](https://aka.ms/ams-v3-java-sdk)|[Java başvurusu](https://aka.ms/ams-v3-java-ref)|
|[Python SDK'sı](https://aka.ms/ams-v3-python-sdk)|[Python başvurusu](https://aka.ms/ams-v3-python-ref)|
|[Node.js SDK'sı](https://aka.ms/ams-v3-nodejs-sdk) |[Node.js başvurusu](/javascript/api/overview/azure/mediaservices/management)| 
|[Go SDK'sı](https://aka.ms/ams-v3-go-sdk) |[Go başvurusu](https://aka.ms/ams-v3-go-ref)|
|[Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)||

### <a name="see-also"></a>Ayrıca bkz.

- [Medya Hizmeti olaylarını içeren EventGrid .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/)
- [Medya Hizmetleri etkinliklerinin tanımları](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/eventgrid/data-plane/Microsoft.Media/stable/2018-01-01/MediaServices.json)

## <a name="azure-media-services-explorer"></a>Azure Media Services Gezgini

[Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE), Medya Hizmetleri hakkında bilgi edinmek isteyen Windows müşterileri tarafından kullanılabilen bir araçtır. AMSE, Medya Hizmetleri ile yükleme, indirme, kodlama, VOD akışı ve canlı içerik yükleme, indirme,kodlama, akış yapan bir Winforms/C# uygulamasıdır. AMSE aracı, medya hizmetlerini herhangi bir kod yazmadan test etmek isteyen istemciler içindir. AMSE kodu, Medya Hizmetleri ile geliştirmek isteyen müşteriler için bir kaynak olarak sağlanır.

AMSE bir Açık Kaynak projesidir, destek topluluk tarafından sağlanır https://github.com/Azure/Azure-Media-Services-Explorer/issues)(sorunlar rapor edilebilir. Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur. Daha fazla bilgi için, Davranış Kuralları opencode@microsoft.com [SSS'ye](https://opensource.microsoft.com/codeofconduct/faq/) bakın veya diğer soru veya yorumlarla iletişim kurun.

## <a name="filtering-ordering-paging-of-media-services-entities"></a>Medya Hizmetleri kuruluşlarının filtreleme, sipariş, sayfalama

Bkz. [Azure Medya Hizmetleri varlıklarınıfiltreleme, sıralama, sayfalama.](entities-overview.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

## <a name="next-steps"></a>Sonraki adımlar

* [Java ile Medya Hizmetlerine Bağlanın](configure-connect-java-howto.md)
* [.NET ile Medya Hizmetlerine Bağlanın](configure-connect-dotnet-howto.md)
* [Node.js ile Medya Hizmetlerine Bağlanın](configure-connect-nodejs-howto.md)
* [Python ile Medya Hizmetlerine Bağlanın](configure-connect-python-howto.md)
