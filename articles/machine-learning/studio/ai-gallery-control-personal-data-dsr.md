---
title: Azure Yapay Zeka Galerisi verilerini yönetme
titleSuffix: ML Studio (classic) - Azure
description: Dışarı aktarma ve ürün içi kullanıcı verilerinizi arabirimi veya yapay ZEKA Galerisi'ni Kataloğu API'sini kullanarak Azure AI Gallery Sil. Bu makalede, nasıl gösterir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 05/25/2018
ms.reviewer: jmartens, mldocs
ms.openlocfilehash: a4ce383959b10836791ea065ffe8a9c243f6ad0d
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77168983"
---
# <a name="view-and-delete-in-product-user-data-from-azure-ai-gallery"></a>Ve Azure AI Gallery ürün içi kullanıcı verilerini silme

Görüntüleyebilir ve ürün içi kullanıcı verilerinizi arabirimi veya yapay ZEKA Galerisi'ni Kataloğu API'sini kullanarak Azure AI Gallery Sil. Bu makalede nasıl yapılacağı açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="view-your-data-in-ai-gallery-with-the-ui"></a>AI Galerisi'nde kullanıcı Arabirimi ile verilerinizi görüntüleyin

Azure AI Gallery Web UI yayımlanan öğeleri görüntüleyebilir. Kullanıcılar, hem genel hem de listelenmemiş çözümler, projeler, denemeler ve yayımlanan diğer öğeleri görüntüleyebilir:

1.  [Azure yapay zeka Galerisi](https://gallery.azure.ai/)oturum açın.
2.  Profil resmi sağ üst köşedeki ve hesap adını, böylece profil sayfanızı yüklemek için tıklayın.
3.  Profil sayfasında listelenmeyen girişleri de dahil olmak üzere galeride yayımlanmış tüm öğeleri görüntüler.

## <a name="use-the-ai-gallery-catalog-api-to-view-your-data"></a>Verilerinizi görüntülemek için yapay ZEKA Galerisi'ni Kataloğu API'sini kullanın.

https://catalog.cortanaanalytics.com/entitiesadresinden erişilebilen AI Galerisi katalog API 'SI aracılığıyla toplanan verileri programlama yoluyla görüntüleyebilirsiniz. Verileri görüntülemek için yazar kimliğinizi gerekir Katalog API aracılığıyla listelenmemiş olan varlıkları görüntülemek için bir erişim belirteci gerekir.

Katalog yanıtları JSON biçiminde döndürülür.

### <a name="get-an-author-id"></a>Bir yazar Kimliğini alın
Yazar Kimliği için Azure AI Gallery yayımlama sırasında kullanılan e-posta adresini temel alır. Değişmez:

1.  [Azure yapay zeka Galerisi](https://gallery.azure.ai/)oturum açın.
2.  Profil resmi sağ üst köşedeki ve hesap adını, böylece profil sayfanızı yüklemek için tıklayın.
3.  Adres çubuğundaki URL, `authorId=`aşağıdaki alfasayısal KIMLIĞI görüntüler. Örneğin, URL için: `https://gallery.azure.ai/Home/Author?authorId=99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`
        
    Yazar KIMLIĞI: `99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`

### <a name="get-your-access-token"></a>Erişim belirteci alma

Katalog API aracılığıyla listelenmemiş olan varlıkları görüntülemek için bir erişim belirteci ihtiyacınız vardır. Bir erişim belirteci, kullanıcılar hala genel varlıklar ve diğer kullanıcı bilgileri görüntüleyebilir.

Erişim belirteci almak için, oturum açıkken tarayıcının katalog API 'sine yaptığı bir HTTP isteğinin `DataLabAccessToken` üst bilgisini incelemeniz gerekir:

1.  [Azure yapay zeka Galerisi](https://gallery.azure.ai/)oturum açın.
2.  Profil resmi sağ üst köşedeki ve hesap adını, böylece profil sayfanızı yüklemek için tıklayın.
3.  F12 tuşuna basarak tarayıcı geliştirici araçları bölmesini açın, ağ sekmesini seçin ve sayfayı yenileyin. 
4. Filtre metin kutusuna yazarak dize *kataloğunda* istekleri filtreleyin.
5.  URL `https://catalog.cortanaanalytics.com/entities`istekler ' de bir GET isteği bulun ve *üstbilgiler* sekmesini seçin. aşağı kaydırarak *istek üst bilgileri* bölümüne gidin.
6.  Üst bilgi `DataLabAccessToken`, alfasayısal bir belirteçtir. Verilerinizin güvenliğini sağlamanıza yardımcı olmak için bu belirteci paylaşmayın.

### <a name="view-user-information"></a>Kullanıcı bilgilerini görüntüleme
Önceki adımlarda aldığınız yazar KIMLIĞINI kullanarak, aşağıdaki URL 'de `[AuthorId]` değiştirerek bir kullanıcının profilindeki bilgileri görüntüleyin:

    https://catalog.cortanaanalytics.com/users/[AuthorID]

Örneğin, bu URL isteği:
    
    https://catalog.cortanaanalytics.com/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA

Gibi bir yanıt döndürür:

    {"entities_count":9,"contribution_score":86.351575190956922,"scored_at":"2018-05-07T14:30:25.9305671+00:00","contributed_at":"2018-05-07T14:26:55.0381756+00:00","created_at":"2017-12-15T00:49:15.6733094+00:00","updated_at":"2017-12-15T00:49:15.6733094+00:00","name":"First Last","slugs":["First-Last"],"tenant_id":"14b2744cf8d6418c87ffddc3f3127242","community_id":"9502630827244d60a1214f250e3bbca7","id":"99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA","_links":{"self":"https://catalog.azureml.net/tenants/14b2744cf8d6418c87ffddc3f3127242/communities/9502630827244d60a1214f250e3bbca7/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA"},"etag":"\"2100d185-0000-0000-0000-5af063010000\""}


### <a name="view-public-entities"></a>Görünüm genel varlıklar

Katalog API 'SI, yayımlanan varlıklar hakkındaki bilgileri doğrudan [AI Galerisi Web sitesinde](https://gallery.azure.ai/)görüntüleyebileceğiniz Azure yapay zeka Galerisi depolar. 

Yayınlanan varlıkları görüntülemek için aşağıdaki URL 'yi ziyaret edin ve `[AuthorId]`, yukarıdaki [bir yazar kimliği Al](#get-an-author-id) bölümünde elde EDILEN yazar kimliğiyle değiştirin.

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '[AuthorId]'

Örneğin:

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA'

### <a name="view-unlisted-and-public-entities"></a>Listede bulunmayan ve ortak olan varlıkları görüntülemek

Bu sorgu, yalnızca genel varlıklar görüntüler. Listelenmemiş olanlar da dahil olmak üzere tüm varlıklarınızı, görüntülemek için erişim sağlamak önceki bölümden alınan belirteci.

1.  [Postman](https://www.getpostman.com)gibi bir araç kullanarak, [erişim belirtecinizi edinme](#get-your-access-token)bölümünde açıklandığı gibi Katalog URL 'sine bir http get isteği oluşturun.
2.  `DataLabAccessToken`adlı bir HTTP istek üst bilgisi oluşturun, bu değer erişim belirtecine ayarlanır.
3.  HTTP isteği gönderin.

> [!TIP]
> Listelenmemiş varlıkları Kataloğu API'sinden yanıtlarındaki görünmüyorsa, kullanıcı geçersiz olabilir ya da erişim belirtecinizin süresi. Azure Yapay Zeka Galerisi oturumunuzu kapatın ve ardından belirteci yenilemek için [erişim belirtecinizi alma](#get-your-access-token) bölümündeki adımları yineleyin. 
