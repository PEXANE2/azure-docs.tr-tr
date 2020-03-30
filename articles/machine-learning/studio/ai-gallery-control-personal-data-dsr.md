---
title: Azure AI Galerisi verilerini yönetme
titleSuffix: ML Studio (classic) - Azure
description: Arayüz veya AI Galeri Kataloğu API'sini kullanarak ürün içi kullanıcı verilerinizi Azure AI Galerisi'nden dışa aktarabilir ve silebilirsiniz. Bu makalede, nasıl gösterir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 05/25/2018
ms.reviewer: jmartens, mldocs
ms.openlocfilehash: 03341b9e663398f2c42266dead0d2dd01e97c3f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204554"
---
# <a name="view-and-delete-in-product-user-data-from-azure-ai-gallery"></a>Azure AI Galerisi'nden ürün içi kullanıcı verilerini görüntüleme ve silme

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Arabirimi veya AI Galeri Kataloğu API'sini kullanarak ürün içi kullanıcı verilerinizi Azure AI Galerisi'nden görüntüleyebilir ve silebilirsiniz. Bu makalede nasıl söyler.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="view-your-data-in-ai-gallery-with-the-ui"></a>UI ile verilerinizi AI Galerisi'nde görüntüleyin

Azure AI Gallery web sitesi web sitesi web sitesinde yayınladığınız öğeleri görüntüleyebilirsiniz. Kullanıcılar hem herkese açık hem de listelenmemiş çözümleri, projeleri, denemeleri ve diğer yayımlanmış öğeleri görüntüleyebilir:

1.    [Azure AI Galerisi'nde](https://gallery.azure.ai/)oturum açın.
2.    Sağ üst köşedeki profil resmine ve ardından profil sayfanızı yüklemek için hesap adını tıklatın.
3.    Profil sayfası, listelenmemiş girişler de dahil olmak üzere galeride yayınlanan tüm öğeleri görüntüler.

## <a name="use-the-ai-gallery-catalog-api-to-view-your-data"></a>Verilerinizi görüntülemek için AI Galeri Kataloğu API'sini kullanın

AI Galeri Katalog API'si aracılığıyla toplanan verileri programlı https://catalog.cortanaanalytics.com/entitiesolarak görüntüleyebilirsiniz. Verileri görüntülemek için Yazar Kimliğinize ihtiyacınız var. Listelenmemiş varlıkları Katalog API'si üzerinden görüntülemek için bir erişim belirteci gerekir.

Katalog yanıtları JSON biçiminde döndürülür.

### <a name="get-an-author-id"></a>Yazar kimliği alma
Yazar kimliği, Azure AI Galerisi'nde yayımlanırken kullanılan e-posta adresini temel almaktadır. Bu değişmez:

1.    [Azure AI Galerisi'nde](https://gallery.azure.ai/)oturum açın.
2.    Sağ üst köşedeki profil resmine ve ardından profil sayfanızı yüklemek için hesap adını tıklatın.
3.    Adres çubuğundaki URL aşağıdaki `authorId=`alfasayısal kimliği görüntüler. Örneğin, URL için:`https://gallery.azure.ai/Home/Author?authorId=99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`
        
    Yazar Kimliği:`99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`

### <a name="get-your-access-token"></a>Erişim jetonunuzu alın

Katalog API'si aracılığıyla listelenmemiş varlıkları görüntülemek için bir erişim belirteci gerekir. Access Jetonu olmadan, kullanıcılar yine de ortak varlıkları ve diğer kullanıcı bilgilerini görüntüleyebilir.

Erişim jetonu almak için, tarayıcının `DataLabAccessToken` oturum açarken Katalog API'sine yaptığı bir HTTP isteğinin üstbilgisini incelemeniz gerekir:

1.    [Azure AI Galerisi'nde](https://gallery.azure.ai/)oturum açın.
2.    Sağ üst köşedeki profil resmine ve ardından profil sayfanızı yüklemek için hesap adını tıklatın.
3.    F12 tuşuna basarak tarayıcı Geliştirici Araçları bölmesini açın, Ağ sekmesini seçin ve sayfayı yenileyin. 
4. Filtre metin kutusuna yazarak dize *kataloğundaki* istekleri filtreleyin.
5.    URL'deki `https://catalog.cortanaanalytics.com/entities`isteklerde GET isteği bulun ve *Üstbilgi* sekmesini *Request Headers* seçin.
6.    Üstbilginin `DataLabAccessToken` altında alfasayısal belirteç var. Verilerinizin güvenliğini sağlamak için bu belirteci paylaşmayın.

### <a name="view-user-information"></a>Kullanıcı bilgilerini görüntüleme
Önceki adımlarda aldığınız yazar kimliğini kullanarak, aşağıdaki URL'yi değiştirerek `[AuthorId]` kullanıcının profilindeki bilgileri görüntüleyin:

    https://catalog.cortanaanalytics.com/users/[AuthorID]

Örneğin, bu URL isteği:
    
    https://catalog.cortanaanalytics.com/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA

Şu gibi bir yanıt verir:

    {"entities_count":9,"contribution_score":86.351575190956922,"scored_at":"2018-05-07T14:30:25.9305671+00:00","contributed_at":"2018-05-07T14:26:55.0381756+00:00","created_at":"2017-12-15T00:49:15.6733094+00:00","updated_at":"2017-12-15T00:49:15.6733094+00:00","name":"First Last","slugs":["First-Last"],"tenant_id":"14b2744cf8d6418c87ffddc3f3127242","community_id":"9502630827244d60a1214f250e3bbca7","id":"99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA","_links":{"self":"https://catalog.azureml.net/tenants/14b2744cf8d6418c87ffddc3f3127242/communities/9502630827244d60a1214f250e3bbca7/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA"},"etag":"\"2100d185-0000-0000-0000-5af063010000\""}


### <a name="view-public-entities"></a>Kamu tüzel kişiliklerini görüntüleme

Catalog API, yayımlanmış varlıklarla ilgili bilgileri doğrudan [AI Galerisi web sitesinde](https://gallery.azure.ai/)de görüntüleyebileceğiniz Azure AI Galerisi'nde saklar. 

Yayımlanmış varlıkları görüntülemek için, yukarıdaki yazar `[AuthorId]` [kimliği](#get-an-author-id) alın'da elde edilen Yazar Kimliği'ni değiştirerek aşağıdaki URL'yi ziyaret edin.

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '[AuthorId]'

Örnek:

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA'

### <a name="view-unlisted-and-public-entities"></a>Liste dışı ve kamu tüzel kişiliklerini görüntüleme

Bu sorgu yalnızca ortak varlıkları görüntüler. Listelenmemiş olanlar da dahil olmak üzere tüm varlıklarınızı görüntülemek için, önceki bölümden elde edilen erişim belirteci sağlayın.

1.    [Postman](https://www.getpostman.com)gibi bir araç kullanarak, [erişim jetonalın](#get-your-access-token)açıklandığı gibi katalog URL'ye bir HTTP GET isteği oluşturun.
2.    Erişim belirteci için `DataLabAccessToken`ayarlanan değerle birlikte, adlı bir HTTP istek üstbilgisi oluşturun.
3.    HTTP isteğini gönderin.

> [!TIP]
> Listelenmemiş varlıklar Katalog API'sinden gelen yanıtlarda görünmüyorsa, kullanıcıgeçersiz veya süresi dolmuş bir erişim jetonu olabilir. Azure AI Galerisi'nden çıkış ve ardından belirteci yenilemek için [erişim belirtecinizi alın](#get-your-access-token) adımlarını tekrarlayın. 
