---
title: Sonuçları kırpmak için güvenlik filtreleri
titleSuffix: Azure Cognitive Search
description: Güvenlik filtrelerini ve kullanıcı kimliklerini kullanarak Azure Bilişsel Arama içeriğinde erişim denetimi.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 24f168f68a60ebb0408b7f1c367039ea5caea6d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72794272"
---
# <a name="security-filters-for-trimming-results-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da sonuçları kırpmak için güvenlik filtreleri

Kullanıcı kimliğine dayalı olarak Azure Bilişsel Arama'da arama sonuçlarını kırpmak için güvenlik filtreleri uygulayabilirsiniz. Bu arama deneyimi genellikle, belgeye izinleri olan ilkeleri içeren bir alanla aramayı isteyen kişinin kimliğinin karşılaştırılmasını gerektirir. Bir eşleşme bulunduğunda, kullanıcı veya asıl (grup veya rol gibi) bu belgeye erişebilir.

Güvenlik filtrelemi elde etmenin bir yolu, eşitlik ifadelerinin karmaşık bir `Id eq 'id1' or Id eq 'id2'`şekilde disjunction yoluyla: örneğin, ve benzeri. Bu yaklaşım hataya yatkındır, bakımı zordur ve listenin yüzlerce veya binlerce değer içerdiği durumlarda sorgu yanıt süresini saniyeler içinde yavaşlatıyor. 

Daha basit ve daha hızlı `search.in` bir yaklaşım işlevi ile. Eşitlik ifadesi `search.in(Id, 'id1, id2, ...')` yerine kullanırsanız, alt saniye yanıt süreleri bekleyebilirsiniz.

Bu makalede, aşağıdaki adımları kullanarak güvenlik filtreleme gerçekleştirmek için nasıl gösterir:
> [!div class="checklist"]
> * Ana tanımlayıcıları içeren bir alan oluşturma 
> * Mevcut belgeleri ilgili ana tanımlayıcılarla itme veya güncelleme
> * Bir arama isteği `search.in` verme`filter`

>[!NOTE]
> Asıl tanımlayıcıları alma işlemi bu belgede yer almaz. Kimlik servis sağlayıcınızdan almalısınız.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, bir [Azure aboneliğiniz,](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) [Azure Bilişsel Arama hizmetiniz](https://docs.microsoft.com/azure/search/search-create-service-portal)ve [Azure Bilişsel Arama Dizini](https://docs.microsoft.com/azure/search/search-create-index-portal)var sayılıyor.  

## <a name="create-security-field"></a>Güvenlik alanı oluşturma

Belgeleriniz, hangi grupların erişimi olduğunu belirten bir alan içermelidir. Bu bilgiler, belgelerin verilen sonuca döndürülen sonuçtan seçildiği veya reddedildiği filtre ölçütleri olur.
Güvenli dosyaların bir dizinine sahip olduğumuzu ve her dosyaya farklı bir kullanıcı kümesi tarafından erişilenleri varsayalım.
1. Alan `group_ids` ekle (burada herhangi bir ad `Collection(Edm.String)`seçebilirsiniz) olarak . Arama sonuçlarının kullanıcının erişimine `true` göre filtrelemesi için alanın bir `filterable` öznitelik ayarladıkolduğundan emin olun. Örneğin, `group_ids` alanı belge `["group_id1, group_id2"]` için "secured_file_b" `file_name` olarak ayarlarsanız, yalnızca grup kimliklerine ait "group_id1" veya "group_id2" olan kullanıcılar dosyaya erişimi okumuştur.
   Alanın `retrievable` özniteliğinin, arama isteğinin `false` bir parçası olarak döndürülmemesi için ayarlandıklarından emin olun.
2. Ayrıca `file_id` bu `file_name` örnek uğruna ekleyin ve alanlar.  

```JSON
{
    "name": "securedfiles",  
    "fields": [
        {"name": "file_id", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "file_name", "type": "Edm.String"},
        {"name": "group_ids", "type": "Collection(Edm.String)", "filterable": true, "retrievable": false}
    ]
}
```

## <a name="pushing-data-into-your-index-using-the-rest-api"></a>REST API'sini kullanarak verileri dizininize itme
  
Dizininizin URL bitiş noktasına bir HTTP POST isteği gönderin. HTTP isteğinin gövdesi eklenecek belgeleri içeren bir JSON nesnesidir:

```
POST https://[search service].search.windows.net/indexes/securedfiles/docs/index?api-version=2019-05-06  
Content-Type: application/json
api-key: [admin key]
```

İstek gövdesinde, belgelerinizin içeriğini belirtin:

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "file_id": "1",
            "file_name": "secured_file_a",
            "group_ids": ["group_id1"]
        },
        {
            "@search.action": "upload",
            "file_id": "2",
            "file_name": "secured_file_b",
            "group_ids": ["group_id1", "group_id2"]
        },
        {
            "@search.action": "upload",
            "file_id": "3",
            "file_name": "secured_file_c",
            "group_ids": ["group_id5", "group_id6"]
        }
    ]
}
```

Varolan bir belgeyi grup listesiyle güncelleştirmeniz gerekiyorsa, `merge` `mergeOrUpload` aşağıdakileri veya eylemi kullanabilirsiniz:

```JSON
{
    "value": [
        {
            "@search.action": "mergeOrUpload",
            "file_id": "3",
            "group_ids": ["group_id7", "group_id8", "group_id9"]
        }
    ]
}
```

Belge ekleme veya güncelleme hakkında tüm ayrıntılar için [belgeleri edit'i](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)okuyabilirsiniz.
   
## <a name="apply-the-security-filter"></a>Güvenlik filtresini uygulama

Belgeleri erişime dayalı `group_ids` olarak kırpmak için, 'group_id1, group_id2,...' bulunan bir filtreyle bir `group_ids/any(g:search.in(g, 'group_id1, group_id2,...'))` arama sorgusu düzenlemeniz gerekir arama isteği verenin ait olduğu gruplardır.
Bu filtre, `group_ids` alanın verilen tanımlayıcılardan birini içerdiği tüm belgelerle eşleşir.
Azure Bilişsel Arama'yı kullanarak belgeleri arama hakkında tüm ayrıntılar için [Arama Belgeleri'ni](https://docs.microsoft.com/rest/api/searchservice/search-documents)okuyabilirsiniz.
Bu örnek, POST isteğini kullanarak belgelerin nasıl arandığını gösterir.

HTTP POST isteğini sorun:

```
POST https://[service name].search.windows.net/indexes/securedfiles/docs/search?api-version=2019-05-06
Content-Type: application/json  
api-key: [admin or query key]
```

İstek gövdesindeki filtreyi belirtin:

```JSON
{
   "filter":"group_ids/any(g:search.in(g, 'group_id1, group_id2'))"  
}
```

Belgeleri "group_id1" veya `group_ids` "group_id2" içeren yerlerde geri almalısınız. Başka bir deyişle, istek verenin erişimi okuduğu belgeleri alırsınız.

```JSON
{
 [
   {
    "@search.score":1.0,
     "file_id":"1",
     "file_name":"secured_file_a",
   },
   {
     "@search.score":1.0,
     "file_id":"2",
     "file_name":"secured_file_b"
   }
 ]
}
```
## <a name="conclusion"></a>Sonuç

Kullanıcı kimliğine ve Azure Bilişsel Arama `search.in()` işlevine göre sonuçları bu şekilde filtreleyebilirsiniz. İstenen kullanıcının her hedef belgeyle ilişkili asıl tanımlayıcılarla eşleşmesi için ilke tanımlayıcıları geçmek için bu işlevi kullanabilirsiniz. Bir arama isteği işlendiğinde, işlev, kullanıcının `search.in` ilkelerinden hiçbirinin erişimi okumadığı arama sonuçlarını filtreler. Ana tanımlayıcılar güvenlik grupları, roller ve hatta kullanıcının kendi kimliği gibi şeyleri temsil edebilir.
 
## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama filtrelerini kullanarak Etkin Dizin kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
+ [Azure Bilişsel Arama işlemlerinde veri güvenliği ve erişim denetimi](search-security-overview.md)