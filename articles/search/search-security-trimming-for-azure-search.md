---
title: Sonuçları kırpma için güvenlik filtreleri-Azure Search
description: Güvenlik filtrelerini ve Kullanıcı kimliklerini kullanarak Azure Search içerikte erişim denetimi.
ms.service: search
ms.topic: conceptual
services: search
ms.date: 05/02/2019
author: brjohnstmsft
ms.author: brjohnst
manager: nitinme
ms.custom: seodec2018
ms.openlocfilehash: 4d1ffa5b29a56d32a4f6a8ccf40f5bafd27795e6
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186494"
---
# <a name="security-filters-for-trimming-results-in-azure-search"></a>Azure Search sonuçları kırpma için güvenlik filtreleri

Kullanıcı kimliğine göre Azure Search arama sonuçlarını kırpmak için güvenlik filtreleri uygulayabilirsiniz. Bu arama deneyimi genellikle, arama isteğini belge üzerinde izinlere sahip olan ilkeleri içeren bir alana karşı istek yapanın kimliğini karşılaştırmaya gerek duyar. Bir eşleşme bulunduğunda, kullanıcının veya sorumlunun (bir grup veya rol gibi) bu belgeye erişimi vardır.

Güvenlik elde etmek için tek yönlü filtreleme karmaşık bir eşitlik ifadeleri ayrım olduğu: Örneğin, `Id eq 'id1' or Id eq 'id2'`, vb. Bu yaklaşım hataya açıktır, bakım zordur ve listenin yüzlerce veya binlerce değer içermesi durumunda sorgu yanıt süresini pek çok saniyeye yavaşlatır. 

Daha basit ve daha hızlı bir yaklaşım `search.in` işlevi kullanmaktır. Bir eşitlik ifadesi yerine kullanıyorsanız, alt saniyelik yanıt sürelerini bekleyebilir. `search.in(Id, 'id1, id2, ...')`

Bu makalede, aşağıdaki adımları kullanarak güvenlik filtrelemeyi nasıl gerçekleştirebileceğiniz gösterilmektedir:
> [!div class="checklist"]
> * Asıl tanımlayıcıları içeren bir alan oluşturun 
> * Mevcut belgeleri ilgili asıl tanımlayıcılarla gönderin veya güncelleştirin
> * İle `search.in` bir arama isteği verme`filter`

>[!NOTE]
> Asıl tanımlayıcıları alma işlemi bu belgede kapsanmıyor. Bunu kimlik hizmeti sağlayıcınızdan almalısınız.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir [Azure aboneliğiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), [Azure Search hizmeti](https://docs.microsoft.com/azure/search/search-create-service-portal)ve [Azure Search dizininiz](https://docs.microsoft.com/azure/search/search-create-index-portal)olduğunu varsaymaktadır.  

## <a name="create-security-field"></a>Güvenlik alanı oluştur

Belgeleriniz hangi grupların erişimi olduğunu belirten bir alan içermelidir. Bu bilgiler, veren olarak döndürülen sonuç kümesinden belgelerin seçildiği veya reddedildiği bir filtre ölçütü olur.
Güvenli dosyalar dizini olduğunu ve her dosyaya farklı bir kullanıcı kümesi tarafından erişilebilmesini varsayalım.
1. Alan `group_ids` ekleyin (burada herhangi bir `Collection(Edm.String)`ad seçebilirsiniz). Arama sonuçlarının, kullanıcının sahip olduğu `filterable` erişime göre filtrelenmesini sağlamak için alanın olarak `true` ayarlanmış bir özniteliği olduğundan emin olun. Örneğin, "secured_file_b" ile `group_ids` `file_name` belge `["group_id1, group_id2"]` için alanını ayarlarsanız, yalnızca "group_id1" veya "group_id2" grup kimliklerine ait olan kullanıcılar dosyaya okuma erişimine sahiptir.
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

Belgelere erişimi temel alarak `group_ids` kırpmak için bir `group_ids/any(g:search.in(g, 'group_id1, group_id2,...'))` filtreye sahip bir arama sorgusu oluşturmanız gerekir, burada ' group_id1, group_id2,... ' , arama isteği verenin ait olduğu gruplarıdır.
Bu filtre, `group_ids` alanın verilen tanımlayıcılardan birini içerdiği tüm belgelerle eşleşir.
Azure Search kullanarak belge arama hakkında tam Ayrıntılar için, [arama belgelerini](https://docs.microsoft.com/rest/api/searchservice/search-documents)okuyabilirsiniz.
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

Bu, Kullanıcı kimliği ve Azure Search `search.in()` işlevine göre sonuçları filtreleyebilmeniz için de kullanılır. Bu işlevi, istenen kullanıcının her bir hedef belge ile ilişkili asıl tanımlayıcılarla eşleşmesi için ilke tanımlayıcılarını geçirmek için kullanabilirsiniz. Bir arama isteği işlendiği zaman, `search.in` işlevi, Kullanıcı sorumlularının hiçbirinin okuma erişimine sahip olmadığı arama sonuçlarını filtreler. Asıl tanımlayıcılar güvenlik grupları, roller ve hatta kullanıcının kendi kimliği gibi şeyleri temsil edebilir.
 
## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search filtrelerini kullanarak kimlik tabanlı erişim denetimi Active Directory](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Search filtreler](search-filters.md)
+ [Azure Search işlemlerinde veri güvenliği ve erişim denetimi](search-security-overview.md)