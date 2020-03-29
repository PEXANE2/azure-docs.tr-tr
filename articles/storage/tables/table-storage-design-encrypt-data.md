---
title: Azure depolama tablosu verilerini şifreleme | Microsoft Dokümanlar
description: Azure depolama alanında tablo veri şifreleme si hakkında bilgi edinin.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/11/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: f56946702011968a0fcb31f6fbecbaacdc89ea42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60326012"
---
# <a name="encrypt-table-data"></a>Tablo verilerini şifreleme
.NET Azure Depolama İstemci Kitaplığı, ekleme ve değiştirme işlemleri için dize varlık özelliklerinin şifrelemesini destekler. Şifrelenmiş dizeleri ikili özellikleri olarak hizmette depolanır ve şifre çözme sonra dizeleri geri dönüştürülür.    

Tablolar için, şifreleme ilkesine ek olarak, kullanıcıların şifrelenecek özellikleri belirtmesi gerekir. Bu, bir [EncryptProperty] özniteliği (TableEntity'den türeyen POCO varlıkları için) veya istek seçeneklerindeki bir şifreleme çözümleyicisi belirterek yapılabilir. Şifreleme çözümleyicisi, bölüm anahtarı, satır anahtarı ve özellik adı alan ve bu özelliğin şifrelenip şifrelenmemesi gerektiğini belirten bir Boolean döndüren bir temsilcidir. Şifreleme sırasında, istemci kitaplığı kabloya yazarken bir özelliği şifreleyip şifrelememeye karar vermek için bu bilgileri kullanır. Temsilci ayrıca özelliklerin nasıl şifrelendirilenleri hakkında mantık olasılığını da sağlar. (Örneğin, X ise, a özelliğini şifreleyin; aksi takdirde A ve B özelliklerini şifreleyin.) Varlıkları okurken veya sorgularken bu bilgileri sağlamak gerekli değildir.

## <a name="merge-support"></a>Birleştirme desteği

Birleştirme şu anda desteklenmiyor. Özelliklerin bir alt kümesi daha önce farklı bir anahtar kullanılarak şifrelenmiş olabileceğinden, yeni özellikleri birleştirme ve meta verileri güncelleştirme, veri kaybına neden olur. Birleştirme, hizmetten önceden varolan varlığı okumak için ek hizmet çağrıları yapmayı veya her ikisi de performans nedenleriyle uygun olmayan özellik başına yeni bir anahtar kullanmayı gerektirir.     

Tablo verilerini şifreleme hakkında daha fazla bilgi için Microsoft [Azure Depolama için İstemci Tarafı Şifreleme ve Azure Anahtar Kasası'na](../common/storage-client-side-encryption.md)bakın.  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım Desenleri](table-storage-design-patterns.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)
