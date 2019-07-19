---
title: Azure Data Factory eşleme veri akışı sütun desenleri
description: Veri akışlarında eşleme Azure Data Factory sütun desenleri kullanarak Genelleştirilmiş veri dönüştürme desenleri oluşturma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: d24988dfd5cbaf20e92c5afbbc39dc0c78e3ef6a
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314848"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Azure Data Factory eşleme veri akışları sütun desenleri

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Birçok Azure Data Factory veri akışı dönüştürmesi "sütun desenleri" fikrini destekler, böylece sabit kodlanmış sütun adları yerine desenleri temel alan şablon sütunları oluşturabilirsiniz. Bu özelliği Ifade Oluşturucu içinde, tam, belirli alan adları istemek yerine dönüştürme için sütunlarla eşleşecek desenler tanımlamak üzere kullanabilirsiniz. Desenler, özellikle metin dosyalarındaki veya NoSQL veritabanlarındaki sütunları değiştirmek durumunda, gelen kaynak alanları sık değiştirilirse yararlıdır. Bu durum bazen "Schema drift" olarak adlandırılır.

![sütun desenleri](media/data-flow/columnpattern2.png "Sütun desenleri")

Sütun desenleri, hem şema DRFT senaryolarını hem de genel senaryoları işlemek için yararlıdır. Her sütun adını tam olarak bildiğiniz koşullarda iyidir. Sütun adı ve sütun veri türünde bir eşleme deseni oluşturabilir ve bu işlemi, deseniniz `name`  &  `type` ile eşleşen veri akışındaki herhangi bir alana karşı gerçekleştirecek bir dönüştürme ifadesi oluşturabilirsiniz.

Desenleri kabul eden bir dönüşüme ifade eklerken, "sütun düzeni Ekle" yi seçin. Sütun desenleri, şema DRFT sütun eşleştirme desenlerine izin verir.

Şablon sütun desenleri oluştururken, giriş veri `$$` akışından eşleşen her alan için bir başvuruyu temsil etmek üzere ifadesinde kullanın.

Ifade Oluşturucu Regex işlevlerinden birini kullanmayı seçerseniz, daha sonra $1, $2, $3... seçeneğini kullanabilirsiniz. Regex deyiminizden eşleşen alt desenlere başvurmak için.

Sütun deseninin bir örneği, bir dizi gelen alanları içeren TOPLAMı kullanmaktır. Toplam SUM hesaplamaları toplam dönüşümde bulunur. Daha sonra, "integer" ile eşleşen alan türlerinin her biriyle TOPLAMı kullanabilir ve sonra deyiminizdeki her eşleştirmeye başvurmak için $ $ kullanabilirsiniz.

## <a name="match-columns"></a>Sütunları Eşleştir
![sütun stili türleri](media/data-flow/pattern2.png "Model türleri")

Sütunları temel alan desenler oluşturmak için, sütun adı, tür, akış veya konum üzerinde eşleştirebilir ve ifade işlevleri ve normal ifadelerle bunların herhangi bir birleşimini kullanabilirsiniz.

![sütun konumu](media/data-flow/position.png "Sütun konumu")

## <a name="next-steps"></a>Sonraki adımlar
Veri dönüştürmeleri için ADF eşleme veri akışı [ifade dili](http://aka.ms/dataflowexpressions) hakkında daha fazla bilgi edinin
