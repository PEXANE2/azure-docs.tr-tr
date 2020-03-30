---
title: Veri akışını haritalamada şema kayması
description: Schema Drift ile Azure Veri Fabrikası'nda esnek Veri Akışları oluşturun
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/12/2019
ms.openlocfilehash: 9daf7973a2e48f866a8d0b93a682851d31dc3af7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74928550"
---
# <a name="schema-drift-in-mapping-data-flow"></a>Veri akışını haritalamada şema kayması

Şema kayması, kaynaklarınızın genellikle meta verileri değiştirdiği bir durumdur. Alanlar, sütunlar ve türleri anında eklenebilir, kaldırılabilir veya değiştirilebilir. Şema kayması için işleme olmadan, veri akışı upstream veri kaynağı değişiklikleri için savunmasız hale gelir. Bu kaynak adlara bağlı olma eğiliminde olduğundan, gelen sütunlar ve alanlar değiştiğinde tipik ETL desenleri başarısız olur.

Şema kaymasına karşı korumak için, bir Veri Mühendisi olarak aşağıdakileri elde etmenize olanak sağlayacak bir veri akışı aracında yer alan tesislerin olması önemlidir:

* Mutable alan adları, veri türleri, değerler ve boyutlar içeren kaynakları tanımlayın
* Sabit kodlanmış alanlar ve değerler yerine veri desenleri ile çalışabilecek dönüşüm parametrelerini tanımla
* Adlandırılmış alanları kullanmak yerine gelen alanları eşleştirmek için desenleri anlayan ifadeler tanımlayın

Azure Veri Fabrikası, veri akışlarınızı yeniden derlemeye gerek kalmadan genel veri dönüştürme mantığı oluşturabilmeniz için yürütmeden yürütmeye değişen esnek şemaları doğal olarak destekler.

Akışınızda şema sürüklenme kabul etmek için veri akışı mimari bir karar vermek gerekir. Bunu yaptığınızda, kaynaklardan şema değişikliklerine karşı koruyabilirsiniz. Ancak, veri akışınız boyunca sütunlarınızın ve türlerinizin erken bağlanmasını kaybedersiniz. Azure Veri Fabrikası şema sürüklenme akışlarını geç bağlayıcı akışlar olarak ele aldığında, dönüşümlerinizi oluşturduğunuzda, sürüklenen sütun adları akış boyunca şema görünümlerinde kullanılamayacaktır.

## <a name="schema-drift-in-source"></a>Kaynakta şema kayması

Kaynak tanımınızdan veri akışınıza gelen sütunlar, kaynak projeksiyonunuzda bulunmadığında "sürüklenmiş" olarak tanımlanır. Kaynak dönüşümünde kaynak projeksiyonunuzu projeksiyon sekmesinden görüntüleyebilirsiniz. Kaynağınız için bir veri kümesi seçtiğinizde, ADF şemı otomatik olarak veri kümesinden alır ve bu veri kümesi şeması tanımından bir proje oluşturur.

Bir kaynak dönüşümünde, şema kayması veri kümesi şemanızı tanımlanmamış okuma sütunları olarak tanımlanır. Şema kayması sağlamak için, kaynak dönüşümünde **şema kaymasına izin ver'i** işaretleyin.

![Şema sürüklenme kaynağı](media/data-flow/schemadrift001.png "Şema sürüklenme kaynağı")

Şema kayması etkinleştirildiğinde, gelen tüm alanlar yürütme sırasında kaynağınızdan okunur ve tüm akıştan Lavabo'ya geçirilir. Varsayılan olarak, *sürüklenen sütunlar*olarak bilinen tüm yeni algılanan sütunlar, bir dize veri türü olarak gelir. Veri akışınızın sürüklenen sütunların veri türlerini otomatik olarak çıkarmasını istiyorsanız, kaynak ayarlarınızda **Hatalı sürüklenen sütun türlerini** kontrol edin.

## <a name="schema-drift-in-sink"></a>Lavaboda şema kayması

Bir lavabo dönüşümünde, şema kayması, lavabo veri şemasında tanımlanan ın üzerine ek sütunlar yazdığınızda elde edilir. Şema kayması sağlamak için, lavabo dönüşümünde **şema kaymasına izin ver'i** kontrol edin.

![Şema sürüklenme lavabo](media/data-flow/schemadrift002.png "Şema sürüklenme lavabo")

Şema kayması etkinse, Eşleme sekmesindeki **Otomatik eşleme** kaydırıcısının açık olduğundan emin olun. Bu kaydırıcı üzerinde, tüm gelen sütunlar hedefinize yazılır. Aksi takdirde sürüklenen sütunlar yazmak için kural tabanlı eşleme kullanmanız gerekir.

![Lavabo otomatik haritalama](media/data-flow/automap.png "Lavabo otomatik haritalama")

## <a name="transforming-drifted-columns"></a>Sürüklenen sütunları dönüştürme

Veri akışınız sütunları sürüklendiğinde, dönüşümlerinizde aşağıdaki yöntemlerle bunlara erişebilirsiniz:

* Bir `byPosition` sütuna ada veya konum numarasına göre açıkça başvurmak için ifadeleri ve `byName` ifadeleri kullanın.
* Türemiş Sütun'da sütun deseni veya Agrega dönüşümünde herhangi bir ad, akış, konum veya tür birleşimiile eşleşecek şekilde bir sütun deseni ekleme
* Sürüklenen sütunları bir desen aracılığıyla sütun diğer adlarla eşleştirmek için Seç veya Başlat dönüşümünde kural tabanlı eşleme ekleme

Sütun desenleri nasıl uygulanacağı hakkında daha fazla bilgi [için, veri akışını eşlemesütun desenleri](concepts-data-flow-column-pattern.md)bakın.

### <a name="map-drifted-columns-quick-action"></a>Harita sürüklenen sütunlar hızlı eylem

Sürüklenen sütunlara açıkça başvurmak için, veri önizleme hızlı eylemi aracılığıyla bu sütunlar için hızlı bir şekilde eşlemeler oluşturabilirsiniz. [Hata ayıklama modu](concepts-data-flow-debug-mode.md) başladıktan sonra Veri Önizleme sekmesine gidin ve veri önizlemesini almak için **Yenile'yi** tıklatın. Veri fabrikası sürüklenen sütunların var olduğunu algılarsa, **Harita Sürüklenen'i** tıklatabilir ve şema görünümlerinde sürüklenen tüm sütunlara başvurmanızı sağlayan türetilmiş bir sütun oluşturabilirsiniz.

![Harita sürüklendi](media/data-flow/mapdrifted1.png "Harita sürüklendi")

Oluşturulan Türemiş Sütun dönüşümünde, sürüklenen her sütun algılanan adı ve veri türüne eşlenir. Yukarıdaki veri önizlemesinde , 'movieId' sütunu bir alıcı olarak algılanır. **Harita Drifted** tıklandıktan sonra, movieId Türetilmiş Sütun olarak tanımlanır `toInteger(byName('movieId'))` ve aşağı dönüşümlerde şema görünümlerine dahil edilir.

![Harita sürüklendi](media/data-flow/mapdrifted2.png "Harita sürüklendi")

## <a name="next-steps"></a>Sonraki adımlar
Veri [Akışı İfade dili,](data-flow-expression-functions.md)"byName" ve "byPosition" dahil olmak üzere sütun desenleri ve şema kayması için ek olanaklar bulacaksınız.
