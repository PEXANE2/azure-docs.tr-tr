---
title: Medya Hizmetleri operasyonları REST API genel bakış | Microsoft Dokümanlar
description: "\"Medya Hizmetleri İşlemleri REST\" API, Medya Hizmetleri hesabında İşler, Varlıklar, Canlı Kanallar ve diğer kaynakları oluşturmak için kullanılır. Bu makalede, Azure Medya Hizmetleri v2 REST API genel bakış sağlar."
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.reviewer: johndeu
ms.openlocfilehash: 597839f633ed2b925b86c5f859a0fb2d3b64dd59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773668"
---
# <a name="media-services-operations-rest-api-overview"></a>Medya Hizmetleri operasyonları REST API genel bakış 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

**Media Services Operations REST** API, Medya Hizmetleri hesabında İşler, Varlıklar, Canlı Kanallar ve diğer kaynakları oluşturmak için kullanılır. Daha fazla bilgi için, [Bkz. Medya Hizmetleri İşlemleri REST API referansı.](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)

Medya Hizmetleri, hem JSON hem de atom+pub XML biçimini kabul eden bir REST API sağlar. Medya Hizmetleri REST API, her istemcinin Medya Hizmetleri'ne bağlanırken göndermesi gereken belirli BIRE üstbilgilerinin yanı sıra isteğe bağlı üstbilgi gerektirir. Aşağıdaki bölümlerde, medya hizmetlerinden istek oluştururken ve yanıt alırken kullanabileceğiniz üstbilgi ve HTTP fiilleri açıklayınız.

Media Services REST API'nin kimlik doğrulaması, [Azure Medya Hizmetleri API'sine REST ile erişmek için Azure AD kimlik doğrulamasını kullan](media-services-rest-connect-with-aad.md) makalesinde özetlenen Azure Active Directory kimlik doğrulaması aracılığıyla yapılır

## <a name="considerations"></a>Dikkat edilmesi gerekenler

REST kullanırken aşağıdaki hususlar geçerlidir.

* Varlıkları sorgularken, public REST v2 sorgu sonuçlarını 1000 sonuçla sınırladığı için, aynı anda döndürülen 1000 varlık sınırı vardır. [Bu .NET örneğinde](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ve [bu REST API örneğinde](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)açıklandığı gibi **Atla** ve **Al** (.NET)/ **üst** (REST) kullanmanız gerekir. 
* JSON kullanırken ve istekte **__metadata** anahtar sözcüğün kullanılacağını belirtirken (örneğin, bağlantılı bir nesneye başvurmak için) **üstbilgisini** [JSON Verbose biçimine](https://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) ayarlamanız gerekir (aşağıdaki örneğe bakın). Odata, siz ayrıntılı olarak ayarlamadığınız sürece, istekteki **__metadata** özelliğini anlamaz.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.19
        Authorization: Bearer <ENCODED JWT TOKEN> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Medya Hizmetleri tarafından desteklenen standart HTTP istek üstbilgisi
Medya Hizmetleri'ne yaptığınız her arama için, isteğinize eklemeniz gereken bir dizi gerekli üstbilgi ve eklemek isteyebileceğin isteğe bağlı üstbilgi kümesi vardır. Aşağıdaki tabloda gerekli üstbilgi listelemektedir:

| Üst bilgi | Tür | Değer |
| --- | --- | --- |
| Yetkilendirme |Taşıyıcı |Taşıyıcı kabul edilen tek yetkilendirme mekanizmasıdır. Değer, Azure Active Directory tarafından sağlanan erişim belirteci de içermelidir. |
| x-ms sürümü |Ondalık |2.17 (veya en son sürüm)|
| DataServiceVersion |Ondalık |3,0 |
| MaxDataServiceVersion |Ondalık |3,0 |

> [!NOTE]
> Medya Hizmetleri REST API'lerini ortaya çıkarmak için OData'yı kullandığından, DataServiceVersion ve MaxDataServiceVersion başlıkları tüm isteklere dahil edilmelidir; ancak, değilse, şu anda Medya Hizmetleri kullanımda olan DataServiceVersion değerinin 3.0 olduğunu varsayar.
> 
> 

Aşağıda isteğe bağlı üstbilgi kümesi vereme

| Üst bilgi | Tür | Değer |
| --- | --- | --- |
| Tarih |RFC 1123 tarihi |İsteğin zaman damgası |
| Kabul Et |İçerik türü |Aşağıdaki gibi yanıt için istenen içerik türü:<p> -uygulama/json;odata=verbose<p> - uygulama/atom+xml<p> Yanıtlar, başarılı bir yanıtın taşıma yükü olarak blob akışını içerdiği blob alma gibi farklı bir içerik türüne sahip olabilir. |
| Kabul-Kodlama |Gzip, söndürmek |GZIP ve DEFLATE kodlama, ne zaman uygulanabilir. Not: Büyük kaynaklar için Medya Hizmetleri bu üstbilgigözardı edebilir ve sıkıştırılmamış verileri döndürebilir. |
| Accept-Language |"en", "es", ve saire. |Yanıt için tercih edilen dili belirtir. |
| Kabul-Charset |"UTF-8" gibi charset türü |Varsayılan değer UTF-8'dir. |
| X-HTTP Yöntemi |HTTP Yöntemi |PUT veya DELETE gibi HTTP yöntemlerini desteklemeyen istemcilerin veya güvenlik duvarlarının GET çağrısı yla tünellenmiş bu yöntemleri kullanmasına izin verir. |
| İçerik Türü |İçerik türü |PUT veya POST isteklerinde istek gövdesinin içerik türü. |
| istemci-istek-id |Dize |Verilen isteği tanımlayan arayanın tanımlı değeri. Belirtilirse, bu değer isteği eşlenebilmek için yanıt iletisine dahil edilir. <p><p>**Önemli**<p>Değerler 2096b (2k) olarak sınırlandırılmalıdır. |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Medya Hizmetleri tarafından desteklenen standart HTTP yanıt başlıkları
Aşağıda, istediğiniz kaynağa ve gerçekleştirmeyi planladığınız eyleme bağlı olarak size döndürülebilecek üstbilgi kümesi verebilirsiniz.

| Üst bilgi | Tür | Değer |
| --- | --- | --- |
| istek-id |Dize |Geçerli işlem için benzersiz bir tanımlayıcı, hizmet oluşturuldu. |
| istemci-istek-id |Dize |Varsa, özgün istekte arayan tarafından belirtilen bir tanımlayıcı. |
| Tarih |RFC 1123 tarihi |İsteğin işlendiği tarih/saat. |
| İçerik Türü |Değişir |Yanıt gövdesinin içerik türü. |
| İçerik Kodlama |Değişir |Gzip veya söndürmek, uygun olarak. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Medya Hizmetleri tarafından desteklenen standart HTTP fiilleri
Aşağıda, HTTP istekleri yapılırken kullanılabilecek HTTP fiillerinin tam listesi verebilirsiniz:

| Fiil | Açıklama |
| --- | --- |
| GET |Nesnenin geçerli değerini döndürür. |
| POST |Sağlanan verilere dayalı bir nesne oluşturur veya bir komut gönderir. |
| PUT |Bir nesneyi değiştirir veya adlandırılmış bir nesne oluşturur (varsa). |
| DELETE |Bir nesneyi siler. |
| Birleştirme |Varolan bir nesneyi adlandırılmış özellik değişiklikleriyle güncelleştirir. |
| HEAD |GET yanıtı için bir nesnenin meta verilerini döndürür. |

## <a name="discover-and-browse-the-media-services-entity-model"></a>Medya Hizmetleri varlık modelini keşfedin ve göz atın
Medya Hizmetleri varlıklarını daha bulunabilir hale getirmek için $metadata işlemi kullanılabilir. Tüm geçerli varlık türlerini, varlık özelliklerini, ilişkilendirmeleri, işlevleri, eylemleri ve benzeri tüm almanızı sağlar. $metadata işlemini Media Services REST API bitiş noktasının sonuna ekleyerek bu bulma hizmetine erişebilirsiniz.

 /api/$metadata.

Bir tarayıcıda meta verileri görüntülemek istiyorsanız URI'nin sonuna "?api-version=2.x" eklemeli veya isteğinize x-ms-sürüm üstbilgisini eklememelisiniz.

## <a name="authenticate-with-media-services-rest-using-azure-active-directory"></a>Azure Active Directory kullanarak Medya Hizmetleri REST ile kimlik doğrulaması

REST API'deki kimlik doğrulaması Azure Active Directory (AAD) aracılığıyla yapılır.
Azure Portalı'ndan Medya Hizmetleri hesabınız için gerekli kimlik doğrulama ayrıntılarını nasıl alacağınız hakkında ayrıntılı bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın.

Azure AD kimlik doğrulamasını kullanarak REST API'sine bağlanan kod yazma yla ilgili ayrıntılar için, [AZURE Media Services API'sine REST ile erişmek için Azure AD kimlik doğrulamasını kullan makalesine](media-services-rest-connect-with-aad.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Media Services REST API ile Azure AD kimlik doğrulamasını nasıl kullanacağınızı öğrenmek [için, REST ile Azure Medya Hizmetleri API'sine erişmek için Azure AD kimlik doğrulamasını kullanın'a](media-services-rest-connect-with-aad.md)bakın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

