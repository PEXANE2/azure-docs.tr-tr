---
title: Standart kurallar altyapı başvurusunu Azure CDN | Microsoft Docs
description: Azure CDN Standart kuralları altyapısı ile eşleşen koşullar ve eylemlerle ilgili başvuru belgeleri.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: 6fb7e704f3d33cff8c29386b8aba9d8289037cbb
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615942"
---
# <a name="azure-cdn-from-microsoft-rules-engine-reference"></a>Microsoft Rules Engine başvurusundan Azure CDN

Bu makalede, Azure Content Delivery Network (CDN) [Standart kuralları altyapısının](cdn-standard-rules-engine.md)kullanılabilir eşleşme koşullarının ve özelliklerinin ayrıntılı açıklamaları listelenmektedir.

Kural altyapısı, belirli istek türlerinin CDN tarafından nasıl işlendiği konusunda son yetkili olacak şekilde tasarlanmıştır.

**Ortak kullanımlar**:

- Özel bir önbellek ilkesini geçersiz kılın veya tanımlayın.
- İstekleri yeniden yönlendirin.
- HTTP isteği ve yanıt üstbilgilerini değiştirme

## <a name="terminology"></a>Terminoloji

Bir kural, [**eşleşme koşulları**](cdn-standard-rules-engine-match-conditions.md)ve [**eylemleri**](cdn-standard-rules-engine-actions.md)kullanılarak tanımlanır. Bu öğeler aşağıdaki çizimde vurgulanır:

 ![CDN kuralları yapısı](./media/cdn-standard-rules-engine-reference/cdn-rules-structure.png)

Her kuralda en fazla 4 eşleşme koşulu ve 3 eylem olabilir. CDN uç noktası başına en fazla 5 kural vardır. Ayrıca, varsayılan olarak **Genel kural**olarak adlandırılan bir kural vardır. Bu, içinde tanımlanan eylemlerin her zaman tetikleyeceği, eşleşme koşulları olmayan bir kuraldır. Bu kural geçerli 5 kural sınırına dahildir.

## <a name="syntax"></a>Sözdizimi

Özel karakterlerin değerlendirilme şekli, eşleşme koşulunun veya actopd 'nin metin değerlerini nasıl işleydiğine göre farklılık gösterir. Bir eşleştirme koşulu veya özelliği, aşağıdaki yollarla metni yorumlayabilir:

1. [**Değişmez değerler**](#literal-values)
2. [**Joker karakter değerleri**](#wildcard-values)


### <a name="literal-values"></a>Değişmez değerler

Bir sabit değer olarak yorumlanan metin,% symbol özel durumuyla, eşleşmesi gereken değerin bir parçası olarak tüm özel karakterleri ele alır. Diğer bir deyişle, yalnızca tam değer (yani, `\'*'\`) bulunduğunda `\'*'\` olarak ayarlanmış bir değişmez değer eşleştirme koşulu yalnızca karşılanır.

URL kodlamasını göstermek için yüzde işareti kullanılır (örneğin, `%20`).

### <a name="wildcard-values"></a>Joker karakter değerleri

Joker karakter değeri olarak yorumlanan metin özel karakterlere ek anlam atar. Aşağıdaki tabloda aşağıdaki karakter kümesinin nasıl yorumlanacağı açıklanmaktadır:

İnde | Açıklama
----------|------------
\ | Bu tabloda belirtilen karakterlerden herhangi birini atlamak için ters eğik çizgi kullanılır. Bir ters eğik çizgi, önüne kaçılması gereken özel karakterden önce belirtilmelidir.<br/>Örneğin, aşağıdaki sözdizimi bir yıldız işaretine çıkar: `\*`
% | URL kodlamasını göstermek için yüzde işareti kullanılır (örneğin, `%20`).
\* | Yıldız işareti bir veya daha fazla karakteri temsil eden bir joker karakterdir.
Uzay | Boşluk karakteri, bir eşleşme koşulunun belirtilen değerlerden veya desenlerden herhangi biri tarafından karşılanamayacağını gösterir.
deeri | Tek bir teklifin özel anlamı yoktur. Ancak, bir değerin değişmez değer olarak değerlendirilip değerlendirilmeyeceğini göstermek için bir dizi tek tırnak kullanılır. Aşağıdaki yollarla kullanılabilir:<br><br/>-Belirtilen değerin karşılaştırma değerinin herhangi bir bölümüyle eşleşmesi durumunda eşleşme koşulunun karşılanmasına izin verir.  Örneğin, `'ma'` aşağıdaki dizelerin herhangi biriyle eşleşir: <br/><br/>/Business/**ma**rathon/Asset.htm<br/>**ma**p. gif<br/>/Business/Template. **ma**p<br /><br />-Özel bir karakterin sabit karakter olarak belirtilmesini sağlar. Örneğin, bir boşluk karakterini bir dizi tek tırnak içinde (yani, `' '` veya `'sample value'`) ayırarak bir sabit alan karakteri belirtebilirsiniz.<br/>-Boş değer belirtilmesini sağlar. Bir dizi tek tırnak (' ') belirterek boş bir değer belirtin.<br /><br/>**Önemli:**<br/>-Belirtilen değer bir joker karakter içermiyorsa, otomatik olarak değişmez değer olarak değerlendirilir, bu da tek tırnak kümesini belirtmek için gerekli değildir.<br/>-Ters eğik çizgi bu tablodaki başka bir karakterle eşleşmezse, bir dizi tek tırnak içinde belirtildiğinde yok sayılır.<br/>-Sabit karakter olarak özel bir karakter belirtmenin diğer bir yolu da bir ters eğik çizgi (yani, `\`) kullanarak kaçış yöntemidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart kurallar altyapısı eşleştirme koşulları](cdn-standard-rules-engine-match-conditions.md)
- [Standart kurallar altyapı eylemleri](cdn-standard-rules-engine-actions.md)
- [Standart kurallar altyapısını kullanarak HTTPS 'yi zorla](cdn-standard-rules-engine.md)
- [Azure CDN genel bakış](cdn-overview.md)
