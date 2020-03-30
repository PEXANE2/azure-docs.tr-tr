---
title: Azure CDN kuralları motor başvurusu | Microsoft Dokümanlar
description: Azure CDN kuralları için başvuru belgeleri motor eşleşme koşulları ve özellikleri.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: aab93204c850223756f28a56ea550f912e28e0d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69996763"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-reference"></a>Verizon Premium kuralları motor başvuru azure CDN

Bu makalede, Azure İçerik Teslim Ağı (CDN) [kuralları altyapısı](cdn-verizon-premium-rules-engine.md)için kullanılabilir eşleç koşulları ve özellikleriayrıntılı açıklamaları listelenmektedir.

Kurallar altyapısı, cdn tarafından belirli istek türlerinin nasıl işlendiğikonusunda son yetkili olacak şekilde tasarlanmıştır.

**Yaygın kullanım alanları**:

- Geçersiz kılın veya özel bir önbellek ilkesi tanımlayın.
- Hassas içerik isteklerini güvenli hale veya reddetme.
- İstekleri yeniden yönlendirin.
- Özel günlük verilerini depolayın.

## <a name="terminology"></a>Terminoloji

Bir kural [**koşullu ifadeler,**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md) [**maç koşulları**](cdn-verizon-premium-rules-engine-reference-match-conditions.md)ve [**özellikleri**](cdn-verizon-premium-rules-engine-reference-features.md)nin kullanımı ile tanımlanır. Bu öğeler aşağıdaki resimde vurgulanır:

 ![CDN eşleşme durumu](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Sözdizimi

Özel karakterlerin işlenir şekilde bir maç durumu veya özelliği metin değerlerini işler şekilde değişir. Bir eşleşme koşulu veya özelliği metni aşağıdaki yollardan biriyle yorumlayabilir:

1. [**Gerçek değerler**](#literal-values)
2. [**Joker karakter değerleri**](#wildcard-values)
3. [**Normal ifadeler**](#regular-expressions)

### <a name="literal-values"></a>Gerçek değerler

Gerçek bir değer olarak yorumlanan metin, % sembolü hariç tüm özel karakterleri eşleşmesi gereken değerin bir parçası olarak değerlendirir. Başka bir deyişle, tam değer `\'*'\` (yani, `\'*'\`) bulunduğunda yalnızca gerçek bir eşleşme koşulu ayarlanır.

URL kodlamasını belirtmek için bir yüzde simgesi `%20`kullanılır (örneğin,).

### <a name="wildcard-values"></a>Joker karakter değerleri

Joker karakter değeri olarak yorumlanan metin, özel karakterlere ek anlam lar atar. Aşağıdaki tabloda aşağıdaki karakter kümesinin nasıl yorumlanacağı açıklanmaktadır:

Karakter | Açıklama
----------|------------
\ | Bu tabloda belirtilen karakterlerden herhangi birini kaçmak için ters eğik çizgi kullanılır. Bir ters eğik çizgi doğrudan kaçması gereken özel karakter önce belirtilmelidir.<br/>Örneğin, aşağıdaki sözdizimi bir yıldız işaretinden kaçar:`\*`
% | URL kodlamasını belirtmek için bir yüzde simgesi `%20`kullanılır (örneğin,).
\* | Yıldız işareti, bir veya daha fazla karakteri temsil eden bir joker karakterdir.
Alan | Boşluk karakteri, eşleşme koşulunun belirtilen değerler veya desenlerden biri tarafından karşılanabileceğini gösterir.
'değer' | Tek bir alıntının özel bir anlamı yoktur. Ancak, bir değerin gerçek bir değer olarak ele alınması gerektiğini belirtmek için tek tırnak bir dizi kullanılır. Aşağıdaki şekillerde kullanılabilir:<br><br/>- Belirtilen değer karşılaştırma değerinin herhangi bir bölümüyle eşleşse, bir eşleşme koşulunun karşılanmasına izin verir.  Örneğin, `'ma'` aşağıdaki dizeleri eşleşir: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/iş/şablon. **ma**p<br /><br />- Özel bir karakterin gerçek bir karakter olarak belirtilmesine izin verir. Örneğin, bir boşluk karakterini tek tırnak içinde (örn. `' '` veya). `'sample value'`<br/>- Boş bir değerin belirtilmesine izin verir. Tek tırnak kümesi (yani '') belirterek boş bir değer belirtin.<br /><br/>**Önemli:**<br/>- Belirtilen değer bir joker karakter içermiyorsa, otomatik olarak gerçek bir değer olarak kabul edilir, bu da tek bir tırnak kümesi belirtmenin gerekli olmadığı anlamına gelir.<br/>- Bir ters eğik çizgi bu tablodaki başka bir karakterden kaçmazsa, tek tırnak içinde belirtildiğinde yoksayılır.<br/>- Bir edebi karakter olarak özel bir karakter belirtmek için başka bir yolu `\`bir ters eğik çizgi kullanarak kaçmak için (yani, ).

### <a name="regular-expressions"></a>Normal ifadeler

Normal ifadeler, metin değeri içinde aranan bir desen tanımlar. Normal ifade gösterimi, çeşitli sembollere belirli anlamlar tanımlar. Aşağıdaki tablo, özel karakterlerin normal ifadeleri destekleyen maç koşulları ve özelliklerine göre nasıl işlenir olduğunu gösterir.

Özel Karakter | Açıklama
------------------|------------
\ | Bir ters çizgi, onu izleyen karakterden kaçar, bu da bu karakterin normal ifade anlamını almak yerine gerçek bir değer olarak ele alınmasına neden olur. Örneğin, aşağıdaki sözdizimi bir yıldız işaretinden kaçar:`\*`
% | Yüzde sembolünün anlamı, kullanımına bağlıdır.<br/><br/> `%{HTTPVariable}`: Bu sözdizimi bir HTTP değişkeni tanımlar.<br/>`%{HTTPVariable%Pattern}`: Bu sözdizimi, bir HTTP değişkenini tanımlamak için ve sınır layıcı olarak bir yüzde sembolü kullanır.<br />`\%`: Yüzde simgesinden kaçmak, bunun gerçek bir değer olarak kullanılmasına veya URL `\%20`kodlamasını (örneğin) göstermesine olanak sağlar.
\* | Yıldız işareti, önceki karakterin sıfır veya daha fazla kez eşleşmesine izin verir.
Alan | Bir boşluk karakteri genellikle gerçek bir karakter olarak kabul edilir.
'değer' | Tek tırnak edebi karakterler olarak kabul edilir. Tek tırnak bir dizi özel bir anlamı yoktur.

Normal ifadeleri destekleyen eşleşen koşullar ve özellikler, Perl Uyumlu Normal İfadeler (PCRE) tarafından tanımlanan desenleri kabul eder.

## <a name="next-steps"></a>Sonraki adımlar

- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kurallar altyapısını kullanarak HTTP davranışını geçersiz kılma](cdn-verizon-premium-rules-engine.md)
- [Azure CDN'ye genel bakış](cdn-overview.md)
