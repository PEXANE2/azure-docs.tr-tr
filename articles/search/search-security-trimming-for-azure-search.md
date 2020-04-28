---
title: Sonuçları kırpma için güvenlik filtreleri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama içerikte güvenlik filtrelerini ve Kullanıcı kimliklerini kullanarak erişim denetimi.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 24f168f68a60ebb0408b7f1c367039ea5caea6d1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "72794272"
---
# <a name="security-filters-for-trimming-results-in-azure-cognitive-search"></a>Azure Bilişsel Arama sonuçları kırpma için güvenlik filtreleri

Arama sonuçlarını kullanıcı kimliğine göre Azure Bilişsel Arama kırpmak için güvenlik filtreleri uygulayabilirsiniz. Bu arama deneyimi genellikle, arama isteğini belge üzerinde izinlere sahip olan ilkeleri içeren bir alana karşı istek yapanın kimliğini karşılaştırmaya gerek duyar. Bir eşleşme bulunduğunda, kullanıcının veya sorumlunun (bir grup veya rol gibi) bu belgeye erişimi vardır.

Güvenlik filtrelemesi elde etmenin bir yolu, eşitlik ifadelerinin karmaşık bir ilişkisini kullanmaktır: Örneğin, `Id eq 'id1' or Id eq 'id2'`, vb. Bu yaklaşım hataya açıktır, bakım zordur ve listenin yüzlerce veya binlerce değer içermesi durumunda sorgu yanıt süresini pek çok saniyeye yavaşlatır. 

Daha basit ve daha hızlı bir yaklaşım `search.in` işlevi kullanmaktır. Bir eşitlik ifadesi `search.in(Id, 'id1, id2, ...')` yerine kullanıyorsanız, alt saniyelik yanıt sürelerini bekleyebilir.

Bu makalede, aşağıdaki adımları kullanarak güvenlik filtrelemeyi nasıl gerçekleştirebileceğiniz gösterilmektedir:
> [!div class="checklist"]
> * Asıl tanımlayıcıları içeren bir alan oluşturun 
> * Mevcut belgeleri ilgili asıl tanımlayıcılarla gönderin veya güncelleştirin
> * İle `search.in` bir arama isteği verme`filter`

>[!NOTE]
> Asıl tanımlayıcıları alma işlemi bu belgede kapsanmıyor. Bunu kimlik hizmeti sağlayıcınızdan almalısınız.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede bir [Azure aboneliğiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), [Azure bilişsel arama hizmeti](https://docs.microsoft.com/azure/search/search-create-service-portal)ve [Azure bilişsel arama dizininiz](https://docs.microsoft.com/azure/search/search-create-index-portal)olduğunu varsaymaktadır.  

## <a name="create-security-field"></a>Güvenlik alanı oluştur

Belgeleriniz hangi grupların erişimi olduğunu belirten bir alan içermelidir. Bu bilgiler, veren olarak döndürülen sonuç kümesinden belgelerin seçildiği veya reddedildiği bir filtre ölçütü olur.
Güvenli dosyalar dizini olduğunu ve her dosyaya farklı bir kullanıcı kümesi tarafından erişilebilmesini varsayalım.
1. Alan `group_ids` ekleyin (burada herhangi bir `Collection(Edm.String)`ad seçebilirsiniz). Arama sonuçlarının, kullanıcının sahip olduğu `filterable` erişime göre filtrelenmesini sağlamak için alanın olarak `true` ayarlanmış bir özniteliği olduğundan emin olun. Örneğin, `group_ids` alanı "secured_file_b" olan `["group_id1, group_id2"]` `file_name` belge için olarak ayarlarsanız, yalnızca "group_id1" veya "group_id2" grup kimliklerine ait olan kullanıcılar dosyaya okuma erişimine sahiptir.
   Alan `retrievable` özniteliğinin, arama isteğinin bir parçası olarak döndürülmemesi için olarak `false` ayarlandığından emin olun.
2. Ayrıca, `file_id` bu `file_name` örneğin sake için ve alanlarını ekleyin.  

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

## <a name="pushing-data-into-your-index-using-the-rest-api"></a>REST API kullanarak dizininizdeki verileri iletme
  
Dizininizin URL uç noktasına bir HTTP POST isteği gönderin. HTTP isteğinin gövdesi, eklenecek belgeleri içeren bir JSON nesnesidir:

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

Mevcut bir belgeyi Grup listesi ile güncelleştirmeniz gerekiyorsa, `merge` ya `mergeOrUpload` da eylemini kullanabilirsiniz:

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

Belge ekleme veya güncelleştirme hakkında ayrıntılı bilgi için [belgeleri düzenleme belgesini](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)okuyabilirsiniz.
   
## <a name="apply-the-security-filter"></a>Güvenlik filtresini uygulama

Belgelere erişimi temel alarak `group_ids` kırpmak için bir `group_ids/any(g:search.in(g, 'group_id1, group_id2,...'))` filtre içeren bir arama sorgusu oluşturmalısınız, burada ' group_id1, group_id2,... ' , arama isteği verenin ait olduğu gruplarıdır.
Bu filtre, `group_ids` alanın verilen tanımlayıcılardan birini içerdiği tüm belgelerle eşleşir.
Azure Bilişsel Arama kullanarak belge arama hakkında ayrıntılı bilgi için, [arama belgelerini](https://docs.microsoft.com/rest/api/searchservice/search-documents)okuyabilirsiniz.
Bu örnekte, bir POST isteği kullanılarak belgelerin nasıl aranacağı gösterilmektedir.

HTTP POST isteğini verme:

```
POST https://[service name].search.windows.net/indexes/securedfiles/docs/search?api-version=2019-05-06
Content-Type: application/json  
api-key: [admin or query key]
```

İstek gövdesinde filtreyi belirtin:

```JSON
{
   "filter":"group_ids/any(g:search.in(g, 'group_id1, group_id2'))"  
}
```

Belgeleri "group_id1" veya "group_id2 `group_ids` " içerdiğinde geri almalısınız. Diğer bir deyişle, istek verenin okuma erişimine sahip olduğu belgeleri alırsınız.

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

Bu, Kullanıcı kimliği ve Azure Bilişsel Arama `search.in()` işlevi temelinde sonuçları filtreleyebilmeniz için de kullanılır. Bu işlevi, istenen kullanıcının her bir hedef belge ile ilişkili asıl tanımlayıcılarla eşleşmesi için ilke tanımlayıcılarını geçirmek için kullanabilirsiniz. Bir arama isteği işlendiği zaman, `search.in` işlevi, Kullanıcı sorumlularının hiçbirinin okuma erişimine sahip olmadığı arama sonuçlarını filtreler. Asıl tanımlayıcılar güvenlik grupları, roller ve hatta kullanıcının kendi kimliği gibi şeyleri temsil edebilir.
 
## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama filtrelerini kullanarak kimlik tabanlı erişim denetimi Active Directory](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Bilişsel Arama filtreler](search-filters.md)
+ [Azure Bilişsel Arama işlemlerinde veri güvenliği ve erişim denetimi](search-security-overview.md)