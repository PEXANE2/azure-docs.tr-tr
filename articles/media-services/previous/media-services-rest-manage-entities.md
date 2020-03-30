---
title: REST ile Medya Hizmetleri kuruluşlarının yönetimi | Microsoft Dokümanlar
description: Bu makalede, REST API ile Medya Hizmetleri varlıklarının nasıl yönetilen gösterilebilir.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 95262a32-0f2a-4286-b9e2-1a1ca6399b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: a03bc24b689df342be40536c26149a7611fc5176
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283322"
---
# <a name="managing-media-services-entities-with-rest"></a>REST ile Medya Hizmetleri varlıklarını yönetme  

> [!div class="op_single_selector"]
> * [Geri kalanı](media-services-rest-manage-entities.md)
> * [.NET](media-services-dotnet-manage-entities.md)
> 
> 

Microsoft Azure Media Services, OData v3 üzerine kurulmuş REST tabanlı bir hizmettir. Varlıkları diğer OData hizmetlerinde eklediğiniz gibi ekleyebilir, sorgulayabilir, güncelleyebilir ve silebilirsiniz. İstisnalar, gerektiğinde çağrılacaktır. OData hakkında daha fazla bilgi için [Açık Veri Protokolü belgelerine](https://www.odata.org/documentation/)bakın.

Bu konu, REST ile Azure Medya Hizmetleri varlıklarını nasıl yönetdiğinizi gösterir.

>[!NOTE]
> 1 Nisan 2017’den itibaren, hesabınızdaki 90 günden eski olan tüm İş kayıtları, toplam kayıt sayısı üst kota sınırının altında olsa bile ilişkili Görev kayıtlarıyla birlikte otomatik olarak silinecektir. Örneğin, 1 Nisan 2017'de, hesabınızdaki 31 Aralık 2016'dan büyük tüm İş kayıtları otomatik olarak silinir. İş/görev bilgilerini arşivlemeniz gerekiyorsa, bu konuda açıklanan kodu kullanabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler  

Medya Hizmetleri'ndeki varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri belirlemeniz gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API Geliştirme kurulumu'na](media-services-rest-how-to-use.md)bakın.

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine nasıl bağlanabileceğiniz hakkında bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

## <a name="adding-entities"></a>Varlık ekleme
Medya Hizmetleri'ndeki her varlık, BIR POST HTTP isteği yle Varlıklar gibi bir varlık kümesine eklenir.

Aşağıdaki örnekte AccessPolicy nasıl oluşturulacak gösterilmektedir.

    POST https://media.windows.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

## <a name="querying-entities"></a>Varlıkları sorgulama
Varlıkları sorgulamak ve listelemek kolaydır ve yalnızca GET HTTP isteği ve isteğe bağlı OData işlemleri içerir.
Aşağıdaki örnek, tüm MediaProcessor varlıklarının listesini alır.

    GET https://media.windows.net/API/MediaProcessors HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Aşağıdaki örneklerde olduğu gibi belirli bir varlıkla ilişkili belirli bir varlık veya tüm varlık kümelerini de alabilirsiniz:

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c')/TaskTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Aşağıdaki örnek, tüm İşlerin yalnızca Devlet mülkiyetini döndürür.

    GET https://media.windows.net/API/Jobs?$select=State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Aşağıdaki örnek, "SampleTemplate" adı ile tüm İş Şablonlarını döndürür.

    GET https://media.windows.net/API/JobTemplates?$filter=startswith(Name,%20'SampleTemplate') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

> [!NOTE]
> $expand işlemi, Medya Hizmetlerinde ve LINQ Hususları'nda (WCF Veri Hizmetleri) açıklanan desteklenmeyen LINQ yöntemlerinde desteklenmez.
> 
> 

## <a name="enumerating-through-large-collections-of-entities"></a>Varlıkların büyük koleksiyonları aracılığıyla sayısal
Varlıkları sorgularken, public REST v2 sorgu sonuçlarını 1000 sonuçla sınırladığı için, aynı anda döndürülen 1000 varlık sınırı vardır. Varlıkların büyük koleksiyonu ile sayısallandırmak için **atla** ve **üst** kullanın. 

Aşağıdaki örnek, ilk 2000 işi atlamak ve sonraki 1000 işi almak için **atlama** ve **üst** kullanımının nasıl kullanılacağını gösterir.  

    GET https://media.windows.net/api/Jobs()?$skip=2000&$top=1000 HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net

## <a name="updating-entities"></a>Varlıkları güncelleştirme
Varlık türüne ve içinde olduğu duruma bağlı olarak, bir YAMA, PUT veya BIRLEŞTIRME HTTP istekleri aracılığıyla bu varlıküzerindeki özellikleri güncelleştirebilirsiniz. Bu işlemler hakkında daha fazla bilgi [için, bkz.](https://msdn.microsoft.com/library/dd541276.aspx)

Aşağıdaki kod örneği, Varlık varlığındaki Ad özelliğinin nasıl güncelleştirilebildiğini gösterir.

    MERGE https://media.windows.net/API/Assets('nb:cid:UUID:80782407-3f87-4e60-a43e-5e4454232f60') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net
    Content-Length: 21
    Expect: 100-continue

    {"Name" : "NewName" }

## <a name="deleting-entities"></a>Varlıkları silme
Varlıklar, Media Services'da DELETE HTTP isteği kullanılarak silinebilir. Varlığa bağlı olarak, varlıkları silme sırası önemli olabilir. Örneğin, Varlıklar gibi varlıklar, Kıymeti silmeden önce söz konusu Varlığa başvuran tüm Konum belirlemecileri iptal etmenizi (veya silmenizi) gerektirir.

Aşağıdaki örnek, bir dosyayı blob depolama alanına yüklemek için kullanılan bir Konum belirleyicinin nasıl silindiğini gösterir.

    DELETE https://media.windows.net/API/Locators('nb:lid:UUID:76dcc8e8-4230-463d-97b0-ce25c41b5c8d') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net
    Content-Length: 0

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

