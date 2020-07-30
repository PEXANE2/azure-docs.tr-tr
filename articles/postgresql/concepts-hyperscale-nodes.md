---
title: Düğümler – Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı 'ndaki bir sunucu grubunda bulunan iki tür düğüm, düzenleyici ve çalışan hakkında bilgi edinin.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/28/2020
ms.openlocfilehash: af743ca56572f507091db01f11d3283294a9e3d5
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87382806"
---
# <a name="nodes-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda düğümler – hiper ölçek (Citus)

Hiper ölçek (Citus) barındırma türü, PostgreSQL için Azure veritabanı sunucularının (düğümler olarak adlandırılır), "paylaşılan öğe olmayan" mimaride birbiriyle koordine etmesine olanak tanır. Bir sunucu grubundaki düğümler toplu olarak daha fazla veri tutar ve tek bir sunucuda mümkün olduğunca daha fazla CPU çekirdeği kullanır. Mimari Ayrıca, sunucu grubuna daha fazla düğüm ekleyerek veritabanının ölçeklendirilmesine de olanak tanır.

## <a name="coordinator-and-workers"></a>Koordinatör ve çalışanlar

Her sunucu grubunda bir düzenleyici düğümü ve birden çok çalışan bulunur. Uygulamalar, sorgularını ilgili çalışanlara aktaran ve sonuçlarını toplayan düzenleyici düğümüne gönderir. Uygulamalar doğrudan çalışanlara bağlanamaz.

Hiper ölçek (Citus), veritabanı yöneticisinin farklı çalışan düğümlerinde farklı satırları depolayarak tabloları *dağıtmasını* sağlar. Dağıtılmış tablolar, hiper ölçek performansının anahtarıdır. Tabloların dağıtılamaması, bunları tamamen düzenleyici düğümüne bırakır ve makineler arası paralelden faydalanabilir.

Dağıtılmış tablolardaki her sorgu için, düzenleyici bunu tek bir çalışan düğümüne yönlendirir ya da gerekli verilerin tek bir düğümde veya birden çok üzerinde olmasına bağlı olarak bir kaç tane üzerinde paralelleştirme yapın. Düzenleyici, meta veri tablolarına danışarak ne yapılacağını belirler. Bu tablolar, çalışan düğümlerinin DNS adlarını ve sistem durumunu ve verilerin düğümler arasında dağıtımını izler.

## <a name="next-steps"></a>Sonraki adımlar
- [Dağıtılmış tablolar](concepts-hyperscale-distributed-data.md) hakkında bilgi edinin
