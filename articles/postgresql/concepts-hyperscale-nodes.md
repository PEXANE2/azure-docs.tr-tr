---
title: Düğümler – Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı 'ndaki bir sunucu grubunda bulunan iki tür düğüm, düzenleyici ve çalışan hakkında bilgi edinin.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 04ebb4298f8a5398b0aa9921d740e3eaacfd8e11
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74974011"
---
# <a name="nodes-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda düğümler – hiper ölçek (Citus)

Hiper ölçek (Citus) barındırma türü, PostgreSQL için Azure veritabanı sunucularının (düğümler olarak adlandırılır), "paylaşılan öğe olmayan" mimaride birbiriyle koordine etmesine olanak tanır. Bir sunucu grubundaki düğümler toplu olarak daha fazla veri tutar ve tek bir sunucuda mümkün olduğunca daha fazla CPU çekirdeği kullanır. Mimari Ayrıca, sunucu grubuna daha fazla düğüm ekleyerek veritabanının ölçeklendirilmesine de olanak tanır.

## <a name="coordinator-and-workers"></a>Koordinatör ve çalışanlar

Her sunucu grubunda bir düzenleyici düğümü ve birden çok çalışan bulunur. Uygulamalar, sorgularını ilgili çalışanlara aktaran ve sonuçlarını toplayan düzenleyici düğümüne gönderir. Uygulamalar doğrudan çalışanlara bağlanamaz.

Her sorgu için, düzenleyici bunu tek bir çalışan düğümüne yönlendirir ya da gerekli verilerin tek bir düğümde veya birden çok üzerinde olmasına bağlı olarak bir kaç tane üzerinde paralelleştirme yapın. Düzenleyici, meta veri tablolarına danışarak ne yapılacağını belirler. Bu tablolar, çalışan düğümlerinin DNS adlarını ve sistem durumunu ve verilerin düğümler arasında dağıtımını izler.

## <a name="next-steps"></a>Sonraki adımlar
- Düğümlerin [Dağıtılmış verileri](concepts-hyperscale-distributed-data.md) nasıl depoleyeceğinizi öğrenin
