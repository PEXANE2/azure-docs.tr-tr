---
title: Azure depolama tablo verilerini şifreleme | Microsoft Docs
description: Azure depolama 'da tablo veri şifrelemesi hakkında bilgi edinin.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/11/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: f56946702011968a0fcb31f6fbecbaacdc89ea42
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "60326012"
---
# <a name="encrypt-table-data"></a>Tablo verilerini şifreleme
.NET Azure depolama Istemci kitaplığı, ekleme ve değiştirme işlemleri için dize varlık özelliklerinin şifrelenmesini destekler. Şifrelenmiş dizeler hizmette ikili özellikler olarak depolanır ve şifre çözme sonrasında dizelere geri dönüştürülür.    

Tablolar için, şifreleme ilkesine ek olarak, kullanıcıların şifrelenecek özellikleri belirtmesi gerekir. Bu işlem, bir [EncryptProperty] özniteliği belirtilerek (TableEntity 'tan türetilmiş POCO varlıkları için) veya istek seçeneklerindeki bir şifreleme çözümleyicisine göre yapılabilir. Şifreleme çözümleyici, Bölüm anahtarını, satır anahtarını ve özellik adını alan ve bu özelliğin şifrelenmesi gerekip gerekmediğini belirten bir Boolean döndüren bir temsilcisidir. Şifreleme sırasında, istemci kitaplığı bu bilgileri, Tel yazarken bir özelliğin şifrelenip şifrelenmeyeceğine karar vermek için kullanır. Temsilci, özelliklerin nasıl şifrelendiğini gösteren mantık olasılığını da sağlar. (Örneğin, X ise, özelliğini şifreleyin; Aksi halde A ve B özelliklerini şifreleyin.) Varlıkları okurken veya sorgularken bu bilgilerin sağlanması gerekli değildir.

## <a name="merge-support"></a>Birleştirme desteği

Birleştirme Şu anda desteklenmiyor. Özelliklerin bir alt kümesi daha önce farklı bir anahtar kullanılarak şifrelendiğinden, yeni özellikleri birleştirmek ve veri kaybına neden olan meta veri sonuçlarını güncellemek yeterlidir. Birleştirme, hizmette önceden var olan varlığı okumak ya da her ikisi de performans nedenleriyle uygun olmayan yeni bir anahtar kullanmak için ek hizmet çağrıları yapılmasını gerektirir.     

Tablo verilerini şifreleme hakkında daha fazla bilgi için bkz. [Microsoft Azure depolama Için Istemci tarafı şifreleme ve Azure Key Vault](../common/storage-client-side-encryption.md).  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo tasarım desenleri](table-storage-design-patterns.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)
