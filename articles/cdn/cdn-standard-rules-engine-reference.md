---
title: Azure CDN için standart kurallar motoru başvurusu | Microsoft Dokümanlar
description: Azure İçerik Teslim Ağı (Azure CDN) için Standart kurallar altyapısındaki eşleşme koşulları ve eylemler için başvuru belgeleri.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: allensu
ms.openlocfilehash: 6d4fa4451c3db3d6f2a506eabd5676d18b0219f4
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259910"
---
# <a name="standard-rules-engine-reference-for-azure-cdn"></a>Azure CDN için standart kural altyapısı başvurusu

Azure İçerik Teslim Ağı (Azure CDN) için [Standart kurallar altyapısında,](cdn-standard-rules-engine.md) bir kural bir veya daha fazla eşleşme koşulu ve bir eylemden oluşur. Bu makalede, Azure CDN için Standart kurallar altyapısında bulunan eşleşme koşulları ve özelliklerinin ayrıntılı açıklamalarını sağlar.

Kurallar altyapısı, belirli istek türlerinin Standart Azure CDN tarafından nasıl işlendiğine ilişkin son yetkili olacak şekilde tasarlanmıştır.

**Kurallar için ortak kullanımalanları:**

- Geçersiz kılın veya özel bir önbellek ilkesi tanımlayın.
- İstekleri yeniden yönlendirin.
- HTTP istek ve yanıt üstbilgilerini değiştirin.

## <a name="terminology"></a>Terminoloji

Kurallar motorunda bir kural tanımlamak [için, maç koşullarını](cdn-standard-rules-engine-match-conditions.md) ve [eylemlerini](cdn-standard-rules-engine-actions.md)ayarlayın:

 ![Azure CDN kuralları yapısı](./media/cdn-standard-rules-engine-reference/cdn-rules-structure.png)

Her kuralın en fazla dört eşleşme koşulu ve üç eylemi olabilir. Her Azure CDN bitiş noktasının en fazla beş kuralı olabilir. 

Bir Azure CDN bitiş noktası için geçerli beş kurallı sınıra dahil edilen varsayılan *genel kuraldır.* Genel kuralın eşleşme koşulları yoktur ve genel bir kuralda tanımlanan eylemler her zaman tetiklenir.

## <a name="syntax"></a>Sözdizimi

Özel karakterlerin bir kuralda nasıl ele alınışları, farklı eşleşme koşullarının ve eylemlerin metin değerlerini nasıl işlediğine bağlı olarak değişir. Bir eşleşme koşulu veya eylem aşağıdaki yollardan biri metni yorumlayabilir:

- [Gerçek değerler](#literal-values)
- [Joker karakter değerleri](#wildcard-values)


### <a name="literal-values"></a>Gerçek değerler

Gerçek bir değer olarak yorumlanan metin, *% sembolü dışındaki* tüm özel karakterleri bir kuralda eşleşmesi gereken değerin bir parçası olarak değerlendirir. Örneğin, yalnızca tam değer `'*'` `'*'` bulunduğunda, gerçek bir eşleşme koşulu ayarlanır.

Url kodlamasını belirtmek için yüzde işareti kullanılır `%20`(örneğin, ).

### <a name="wildcard-values"></a>Joker karakter değerleri

Joker karakter değeri olarak yorumlanan metin, özel karakterlere ek anlam lar atar. Aşağıdaki tabloda, belirli özel karakterlerin Standart kurallar altyapısında nasıl yorumlanacağı açıklanmaktadır:

Karakter | Açıklama
----------|------------
\ | Bu tabloda belirtilen karakterlerden herhangi birini kaçmak için ters eğik çizgi kullanılır. Bir ters eğik çizgi doğrudan kaçması gereken özel karakter önce belirtilmelidir. Örneğin, aşağıdaki sözdizimi bir yıldız işaretinden kaçar:`\*`
% | Url kodlamasını belirtmek için yüzde işareti kullanılır `%20`(örneğin, ).
\* | Yıldız işareti, bir veya daha fazla karakteri temsil eden bir joker karakterdir.
space | Boşluk karakteri, eşleşme koşulunun belirtilen değerler veya desenlerden biri tarafından karşılanabileceğini gösterir.
tek tırnak işaretleri | Tek bir tırnak işaretinin özel bir anlamı yoktur. Ancak, tek tırnak işaretleri kümesi, bir değerin gerçek bir değer olarak ele alınması gerektiğini gösterir. Tek tırnak işaretleri aşağıdaki şekillerde kullanılabilir:<ul><li>Belirtilen değer karşılaştırma değerinin herhangi bir bölümüyle eşleşse, bir eşleşme koşulunun karşılanmasına izin vermek için.  Örneğin, `'ma'` aşağıdaki dizeleri eşleşir: <ul><li>/business/**ma**rathon/asset.htm</li><li>**ma**p.gif</li><li>/iş/şablon. **ma**p</li></ul><li>Özel bir karakterin gerçek bir karakter olarak belirtilmesine izin vermek. Örneğin, bir boşluk karakterini tek tırnak işaretleri kümesine (veya)`' '` `'<sample value>'`ekleyerek gerçek bir boşluk karakteri belirtebilirsiniz.</li><li>Boş bir değerin belirtilmesine izin vermek için. Tek tırnak işaretleri kümesi ( '' belirterek boş bir değer**belirtin.**</li></ul>**Önemli**:<br /><ul><li>Belirtilen değer joker karakter içermiyorsa, değer otomatik olarak gerçek bir değer olarak kabul edilir. Gerçek bir değer için tek tırnak işaretleri kümesi belirtmeniz gerekmez.</li><li>Bu tablodaki başka bir karakterden kaçmak için ters eğik çizgi kullanılmazsa, tek tırnak işareti kümesinde belirtildiğinde ters eğik çizgi göz ardı edilir.</li><li>Bir edebi karakter olarak özel bir karakter belirtmenin başka bir yolu`\`bir ters eğik çizgi kullanarak kaçmaktır ( ).</li></ul>

## <a name="next-steps"></a>Sonraki adımlar

- [Standart kurallar motorundaki koşulları eşleştirme](cdn-standard-rules-engine-match-conditions.md)
- [Standart kurallar altyapısındaki eylemler](cdn-standard-rules-engine-actions.md)
- [Standart kural altyapısını kullanarak HTTPS'yi zorlama](cdn-standard-rules-engine.md)
- [Azure CDN'ye genel bakış](cdn-overview.md)
