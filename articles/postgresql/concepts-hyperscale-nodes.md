---
title: Düğümler – Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: PostgreSQL için Azure Veritabanı'ndaki bir sunucu grubunda iki tür düğüm, koordinatör ve işçi hakkında bilgi edinin.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 04ebb4298f8a5398b0aa9921d740e3eaacfd8e11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974011"
---
# <a name="nodes-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure Veritabanında Düğümler – Hyperscale (Citus)

Hyperscale (Citus) barındırma türü, PostgreSQL sunucuları için Azure Veritabanı'nın (düğüm olarak adlandırılır) "paylaşılan hiçbir şey" mimarisinde birbirleriyle eşgüdüm kurmasına olanak tanır. Bir sunucu grubundaki düğümler topluca daha fazla veri tutar ve tek bir sunucuda mümkün olandan daha fazla CPU çekirdeği kullanır. Mimari ayrıca, sunucu grubuna daha fazla düğüm ekleyerek veritabanının ölçeklendirmesine de olanak tanır.

## <a name="coordinator-and-workers"></a>Koordinatör ve çalışanlar

Her sunucu grubunun bir koordinatör düğümü ve birden çok çalışanı vardır. Başvurular sorgularını koordinatör düğüme gönderir, bu da ilgili çalışanlara iletir ve sonuçlarını birikir. Uygulamalar doğrudan işçilere bağlanabilir.

Her sorgu için koordinatör, onu tek bir alt düğüme yönlendirir veya gerekli verilerin tek bir düğümde mi yoksa birden çok düğümde mi yaşadığına bağlı olarak birkaç adedi paralelleştirir. Koordinatör, meta veri tablolarına danışarak ne yapacağına karar verir. Bu tablolar, DNS adlarını ve çalışan düğümlerin sistem durumunu ve düğümler arasında veri dağılımını izler.

## <a name="next-steps"></a>Sonraki adımlar
- Düğümlerin [dağıtılmış verileri](concepts-hyperscale-distributed-data.md) nasıl depoladığını öğrenin
