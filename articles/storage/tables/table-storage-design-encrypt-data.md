---
title: Azure depolama tablo verilerini şifreleme | Microsoft Docs
description: Azure depolama 'da tablo veri şifrelemesi hakkında bilgi edinin. .NET Azure Storage Istemci kitaplığı, ekleme ve değiştirme işlemleri için dize varlıklarını şifrelemenizi sağlar.
services: storage
author: tamram
ms.author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/11/2018
ms.subservice: tables
ms.openlocfilehash: b921be718bfeb5eb95d4a802fb4d2a8cdd0946c1
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88236786"
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
