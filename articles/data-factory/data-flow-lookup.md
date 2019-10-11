---
title: Azure Data Factory eşleme veri akışı arama dönüşümü
description: Azure Data Factory eşleme veri akışı arama dönüşümü
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/03/2019
ms.openlocfilehash: d762bddbe098e30cbf9e9c02da3c06073a358b12
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72249245"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure Data Factory eşleme veri akışı arama dönüşümü

Veri akışınıza başka bir kaynaktan başvuru verileri eklemek için arama ' yı kullanın. Arama dönüşümü, başvuru tablonuzu işaret eden ve anahtar alanları ile eşleşen tanımlı bir kaynak gerektirir.

![Arama dönüşümü](media/data-flow/lookup1.png "arama")

Gelen akış alanları ve başvuru kaynağındaki alanlar arasında eşleştirmek istediğiniz anahtar alanları seçin. İlk olarak, arama için sağ taraf olarak kullanmak üzere veri akışı tasarım tuvalinde yeni bir kaynak oluşturmuş olmanız gerekir.

Eşleşmeler bulunduğunda, başvuru kaynağından elde edilen satırlar ve sütunlar veri akışınıza eklenecektir. Veri akışınız sonunda, havuzunuzu eklemek istediğiniz ilgilendiğiniz alanları seçebilirsiniz. Alternatif olarak, yalnızca saklamak istediğiniz her iki akıştaki alanları tutmak üzere alan listesini ayıklamak için aramalarınızdan sonra bir seçme dönüşümü kullanın.

Arama dönüştürmesi, bir sol dış birleştirmenin eşdeğerini uygular. Bu nedenle, sol kaynak birleştirmenize ait tüm satırları sağ taraftaki eşleşmeler ile görürsünüz. Aramaınızda birden fazla eşleşen değer varsa veya arama ifadesini özelleştirmek isterseniz, bir JOIN dönüşümüne geçiş yapmak ve çapraz bir birleşimin kullanılması tercih edilir. Bu, yürütmeyle ilgili olası Kartezyen ürün Hatalarını ortadan kaldırmak için kullanılır.

## <a name="match--no-match"></a>Eşleşme/eşleşme yok

Arama dönüşümünüzü tamamladıktan sonra, aramanın bir satır eşleşmesi ile sonuçlanıp sonuçlanmadığını temel alarak mantığınızdaki diğer seçimleri yapmak için `isMatch()` ifade işlevini kullanarak her bir eşleşme satırının sonuçlarını incelemek üzere sonraki dönüşümleri kullanabilirsiniz.

## <a name="optimizations"></a>İyileştirmeleri

Data Factory, veri akışları ölçekli Spark ortamlarında yürütülür. Veri kümeniz çalışan düğümü bellek alanına uyabiliyorsanız, arama performansınızı iyileştirebiliriz.

![Yayına katılmayı]yayınla(media/data-flow/broadcast.png "katılması")

### <a name="broadcast-join"></a>Yayın katılımı

Tüm veri kümesini arama ilişkisinin herhangi bir tarafından belleğe göndermek için, ADF isteği için sol ve/veya sağ taraf yayın katılımı ' nı seçin. Daha küçük veri kümeleri için bu, arama performansınızı büyük ölçüde iyileştirebilir.

### <a name="data-partitioning"></a>Veri bölümleme

Ayrıca, bir çalışan başına belleğe daha iyi uyum sağlayan veri kümeleri oluşturmak için arama dönüşümünün En Iyi sekmesinde "bölümlendirme ayarla" ' yı seçerek verilerinizin bölümlenmesini belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[JOIN](data-flow-join.md) ve [Exists](data-flow-exists.md) dönüştürmeleri ADF eşleme veri akışlarında benzer görevleri gerçekleştirir. Sonraki Dönüştürmelere göz atın.
