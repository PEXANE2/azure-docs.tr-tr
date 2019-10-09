---
title: Eşleme veri akışı 'nda türetilmiş sütun dönüşümü-Azure Data Factory | Microsoft Docs
description: Eşleme veri akışı türetilmiş sütun dönüşümüyle birlikte Azure Data Factory ölçekli verileri nasıl dönüştürebileceğinizi öğrenin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: aacd6f1799f1813e168bd04e78f18cf60ad5243f
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72026839"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Eşleme veri akışında türetilmiş sütun dönüşümü

Veri akışınızda yeni sütunlar oluşturmak veya mevcut alanları değiştirmek için türetilmiş sütun dönüşümünü kullanın.

## <a name="derived-column-settings"></a>Türetilmiş sütun ayarları

Varolan bir sütunu geçersiz kılmak için sütun açılan listesinden seçin. Aksi takdirde, sütun seçim alanını bir metin kutusu olarak kullanın ve yeni sütunun adını yazın. Türetilmiş sütunun ifadesini oluşturmak için, ' ifadeyi girin ' kutusuna tıklayarak [veri akışı Ifade oluşturucuyu](concepts-data-flow-expression-builder.md)açın.

Türetilmiş ![sütun ayarları](media/data-flow/dc1.png "türetilmiş sütun ayarları")

Başka türetilmiş sütunlar eklemek için, varolan bir türetilmiş sütunun üzerine gelin ve ' + ' seçeneğine tıklayın. Sonra ' sütun Ekle ' veya ' sütun Ekle ' yi seçin. Sütun adları kaynaklarınızdan değişken ise, sütun desenleri yararlı olabilir. Daha fazla bilgi için bkz. [sütun desenleri](concepts-data-flow-column-pattern.md).

![Yeni türetilmiş sütun seçimi](media/data-flow/columnpattern.png "yeni türetilmiş sütun seçimi")

## <a name="next-steps"></a>Sonraki adımlar

- [Eşleme veri akışı ifade dili](data-flow-expression-functions.md)hakkında daha fazla bilgi edinin.
