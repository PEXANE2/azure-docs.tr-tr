---
title: Azure Data Factory eşleme veri akışı arama dönüşümü
description: Azure Data Factory eşleme veri akışı arama dönüşümü
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: ef72b7aed12afd1cee47b11bc7584d1e53bf2af5
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029333"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure Data Factory eşleme veri akışı arama dönüşümü



Veri akışınıza başka bir kaynaktan başvuru verileri eklemek için arama ' yı kullanın. Arama dönüşümü, başvuru tablonuzu işaret eden ve anahtar alanları ile eşleşen tanımlı bir kaynak gerektirir.

![Arama dönüşümü](media/data-flow/lookup1.png "arama")

Gelen akış alanları ve başvuru kaynağındaki alanlar arasında eşleştirmek istediğiniz anahtar alanları seçin. İlk olarak, arama için sağ taraf olarak kullanmak üzere veri akışı tasarım tuvalinde yeni bir kaynak oluşturmuş olmanız gerekir.

Eşleşmeler bulunduğunda, başvuru kaynağından elde edilen satırlar ve sütunlar veri akışınıza eklenecektir. Veri akışınız sonunda, havuzunuzu eklemek istediğiniz ilgilendiğiniz alanları seçebilirsiniz.

## <a name="match--no-match"></a>Eşleşme/eşleşme yok

Arama dönüşümünüzü tamamladıktan sonra, aramanın bir satır eşleşmesi ile sonuçlanıp sonuçlanmadığını temel alarak mantığınızdaki diğer seçimleri yapmak için `isMatch()` ifade işlevini kullanarak her bir eşleşme satırının sonuçlarını incelemek üzere sonraki dönüşümleri kullanabilirsiniz.

## <a name="optimizations"></a>İyileştirmeleri

Data Factory, veri akışları ölçekli Spark ortamlarında yürütülür. Veri kümeniz çalışan düğümü bellek alanına uyabiliyorsanız, arama performansınızı iyileştirebiliriz.

![Yayına katılmayı]yayınla(media/data-flow/broadcast.png "katılması")

### <a name="broadcast-join"></a>Yayın katılımı

Tüm veri kümesini arama ilişkisinin herhangi bir tarafından belleğe göndermek için, ADF isteği için sol ve/veya sağ taraf yayın katılımı ' nı seçin.

### <a name="data-partitioning"></a>Veri bölümleme

Ayrıca, bir çalışan başına belleğe daha iyi uyum sağlayan veri kümeleri oluşturmak için arama dönüşümünün En Iyi sekmesinde "bölümlendirme ayarla" ' yı seçerek verilerinizin bölümlenmesini belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[JOIN](data-flow-join.md) ve [Exists](data-flow-exists.md) dönüştürmeleri ADF eşleme veri akışlarında benzer görevleri gerçekleştirir. Sonraki Dönüştürmelere göz atın.
