---
title: include dosyası
description: include dosyası
author: HeidiSteen
ms.service: cognitive-search
ms.topic: include
ms.date: 05/06/2019
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: ee430241173a6c19e2a32e176f28411631d9cb19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80272960"
---
Abonelik içinde birden çok hizmet oluşturabilirsiniz. Her biri belirli bir katmanda sağlanabilir. Yalnızca her katmanda izin verilen hizmet sayısıyla sınırlıdırsınız. Örneğin, Temel katmanda en fazla 12 hizmet ve aynı abonelik içinde S1 katmanında 12 hizmet oluşturabilirsiniz. Katmanlar hakkında daha fazla bilgi için azure [Bilişsel Arama için bir SKU veya katman seçin'e](../articles/search/search-sku-tier.md)bakın.

Talep üzerine maksimum hizmet limitleri yükseltilebilir. Aynı abonelik içinde daha fazla hizmete ihtiyacınız varsa Azure Desteği'ne başvurun.

| Kaynak            | Ücretsiz<sup>1</sup> | Temel | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| Maksimum hizmetler    |1     | 16    | 16  | 8  | 6  | 6     | 6  | 6  |
| Arama birimlerinde maksimum ölçek (SU)<sup>2</sup> |Yok |3 SU |36 SU |36 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> Ücretsiz paylaşılan değil, adanmış, kaynaklara dayanmaktadır. Ölçeklendirme paylaşılan kaynaklarda desteklenmez.

<sup>2</sup> Arama birimleri, *çoğaltma* veya *bölüm*olarak ayrılan fatura birimleridir. Depolama, dizin oluşturma ve sorgu işlemleri için her iki kaynağa ihtiyacınız var. SU hesaplamaları hakkında daha fazla bilgi edinmek [için sorgu ve dizin iş yükleri için Ölçek kaynak düzeylerine](../articles/search/search-capacity-planning.md)bakın. 