---
title: Azure Blockchain Belirteçleri şablonları
description: Azure Blockchain Belirteçleri şablonları, genel muhasebe tabanlı belirteçlerin oluşturulmasını ve dağıtılmasını basitleştiren standartlaştırılmış ve yeniden kullanılabilir şablonlardır.
ms.date: 11/04/2019
ms.topic: conceptual
ms.reviewer: brendal
ms.openlocfilehash: 9600a6a251552acd319cc68d2bd281584d65546d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252213"
---
# <a name="azure-blockchain-tokens-templates"></a>Azure Blockchain Belirteçleri şablonları

[!INCLUDE [Preview note](./includes/preview.md)]

Azure Blockchain Belirteçleri şablonu, genel muhasebe tabanlı belirteçlerin oluşturulmasını ve dağıtılmasını kolaylaştıran standart laştırılmış ve yeniden kullanılabilir bir şablondur. Şablon, [Token Taksonomi Çerçevesi (TTF)](overview.md#token-taxonomy-framework) dilbilgisine dayalı bir formülden oluşur. Dilbilgisi, temel belirteç türünü ve belirteç için davranış kümesini kapsar.  

Örneğin, **τ{d,m,b,r}** belirteç şablonu, alt bölünebilir, mintable, burnable ve rol desteği ne kadar fungible temel belirteci açıklar.
  
## <a name="base-token-types"></a>Temel belirteç türleri

Belirli bir kıymetiniz için genel muhasebe tabanlı belirteci tanımlarken ve oluştururken, hangi temel belirteç kullanılacağını göz önünde bulundurmanız önemlidir.

### <a name="fungible"></a>Fungible

Fungible belirteçleri (τF) aynı sınıf veya dizi de olduğu sürece birbirleri ile değiştirilebilir değeri vardır. Bir belirteç, başka bir belirteçle aynı değere veya belirli bir belirteç miktarıyla aynı değere sahiptir. Örneğin, bir dolar fungible bir belirteçtir. Eğer iki kişi her biri bir dolar banknot tutuyorsa, bu dolar banknotlarını sonuç olarak değiştirebilirler. Dolar banknotları eşit değere sahiptir. 

### <a name="non-fungible"></a>Fungible olmayan

Fungible olmayan belirteçleri (τN) genellikle farklı değerlere sahip olduğu gibi aynı türdeki diğer belirteçleri ile değiştirilebilir değildir. Örneğin, bir özellik başlığı fungible olmayan bir belirteçtir. Bir apartman kompleksinde iki farklı daireiçin mülkiyet başlıkları mutlaka eşit değerde değildir, birimin konumu ya da birim üzerinde hangi katta nedeniyle. İki özellik unvan belirteçlerinin algılanan değeri eşit değildir.

### <a name="hybrid"></a>Karma

Hibrit belirteçler, hem fungible belirteçleri hem de fungible belirteçleri bileşenlerine sahip belirteçleri vardır. Karma belirteç, diğer belirteç türünden bir sınıfa sahip bir temel belirteç türüdür.

#### <a name="hybrid-non-fungible-base-with-fungible-segments"></a>Fungible segmentleri ile hibrid non fungible tabanı

Fungible segmentleri belirteci ile bir hibrid olmayan fungible baz fungible belirteçleri ile olmayan bir fungible tabanı vardır.
Örneğin, konser bileti, konserin tarih ve saatinin fungible olmayan temel belirteci olduğu melez bir belirteçtir. Verilen konser için çeşitli oturma bölümlerinde bilet fungible belirteçleri ile segmentleri vardır. Biletler kendi oturma bölümlerinde değiştirilebilir, ancak bölümler arasında değil.

#### <a name="hybrid-fungible-base-with-non-fungible-segments"></a>Fungible olmayan segmentleri ile hibrid fungible baz

Olmayan bir fungible segmentleri belirteci ile bir hibrid fungible baz olmayan fungible belirteç leri ile fungible tabanı vardır. Örneğin, ipotek destekli güvenlik, birden çok sahibinin birçok sahip arasında bölünmüş olan fungible tabanı olan melez bir belirteçtir. Güvenlik değiştirilebilir. Bireysel ipotek belirli ipotek destekli güvenlik temsil fungible olmayan segmentleri vardır.

## <a name="token-behaviors"></a>Belirteç davranışları

Belirteç davranışı, belirteç lerin özelliklerini veya kısıtlamalarını tanımlar. Davranış belirteç tanımının bir parçası olan destekleyen özellikleri içerir. Davranışlar tüm belirteç türlerine veya sadece bir taneye uygulanabilir. Davranışlar, davranışın etkilerine bağlı olarak dahili veya harici olabilir. İç davranış, belirteç üzerindeki özellikleri etkinleştirir veya kısıtlar. Harici bir davranış, davranışın dış aktörden çağrılmasını sağlar veya kısıtlar.

Token Taksonomi Çerçevesi (TTF) belirteç davranışları desteklenen Azure Blockchain Belirteçleri hakkında daha fazla bilgi için [belirteç birleştirme özelliğine](composability.md)bakın.

## <a name="pre-built-token-templates"></a>Önceden oluşturulmuş belirteç şablonları

Azure Blockchain Belirteçleri, değiştirilmeden kullanılabilen önceden oluşturulmuş dört belirteç şablonu sağlar. Belirteçlerinizi hızla oluşturmaya, dağıtmaya ve yönetmeye başlamak için çoğu kullanım örneğinde önceden oluşturulmuş bu şablonları arayabilirsiniz.

### <a name="commodity-tokens"></a>Emtia belirteçleri

Emtia belirteçleri tutarlı bir değere sahiptir ve devredilebilir. Örneğin, bir varil petrol veya bir enerji birimi.

**τF{~d,t,m,b,r}** - fungible, bütün, devredilebilir, mintable, yanıcı ve rol desteğine sahip

Birçok blockchain senaryosu, tedarik zinciri veya birden çok kuruluş arasında şeffaflık ve görünürlük gerektirir. Emtia belirteçleri bu yaygın kullanım durumlarına dayanır. Belirteçleri değiştirilebilir ve tutarlıdır. Emtia belirteci şablonu esnektir ve meta verilerle özelleştirilebilir.

### <a name="qualified-tokens"></a>Nitelikli jetonlar

Nitelikli belirteçler kazanılan bir şeyi temsil eder ve genellikle tek bir varlıkla ilişkilidir ve aktarılamaz. Örneğin, bir diploma veya park ihlali.

**τN{s,~t}** - fungible olmayan, singleton ve devredilemez

Çeşitli denetim ve attestation senaryoları belirteç sahipliğideğiştirilemez gerektirir. Derneğin iyi veya kötü olup olmadığı konusunda nitelikli bir belirteç sağlama gereksinimi olan bir dizi kullanım örnekleri vardır.

### <a name="asset-tokens"></a>Varlık belirteçleri

Kıymet belirteçleri maddeye bağlı benzersiz bir değere sahiptir ve emtiaya alınmaz. Örneğin, bir müze eser veya bir özellik başlığı.

**τN{s,t}** - fungible olmayan, singleton ve aktarılabilir

Varlık belirteçleri emtia belirteçleri ile karıştırılabilir. İki belirteç arasındaki en büyük fark, varlık belirteçlerinin doğal olarak benzersiz olması ve değerin bu belirteç türünden bağımsız olmasıdır. Örneğin, köklü bir sanatçı tarafından yağlıboya resim gibi bir sanat eseri bir varlık belirtecidir. Ancak, Mona Lisa bir sanat baskı bir emtia belirteci olarak kabul edilir. Benzer şekilde, değer mülkün öznel niteliklerinde var olduğundan, özellik başlığı bir varlık belirtecidir.

### <a name="ticket-tokens"></a>Bilet jetonları

Bilet belirteçleri tutarlı bir değere sahiptir ancak genellikle süresi dolur. Örneğin, bir uçak bileti.

**τN{m,b,r}** - fungible olmayan, mintable, yanıcı ve rol desteğine sahip.

Bilet belirteçleri genellikle onları normal bir emtia belirteci farklı kılan bir son kullanma tarihi vardır. Örneğin, uçak bileti, konser bileti veya spor biletinin tümü, belirli kullanım tarihlerine sahip atanmış oturma seçeneklerine sahiptir. Biletlerin tarih ler veya oturma alanları arasında kolayca değiş tokuş edilemeyeceğiniz.

## <a name="next-steps"></a>Sonraki adımlar

Senaryonuz için daha fazla esneklik gerektiriyorsanız, [belirteç birleştirme özelliğini](composability.md)kullanarak kendi belirteç şablonlarınızı oluşturma hakkında bilgi edinin.
