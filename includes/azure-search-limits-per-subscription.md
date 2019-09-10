---
title: include dosyası
description: include dosyası
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 05/06/2019
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 1e147e8bd9260cd1ece60b70641968a229995ec1
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "67188750"
---
Bir abonelik içinde birden çok hizmet oluşturabilirsiniz. Her biri, belirli bir katmanda sağlanabilir. Yalnızca her katmanda izin verilen hizmet sayısıyla sınırlı olursunuz. Örneğin, temel katmanda 12 ' ye kadar hizmet ve aynı abonelik içindeki S1 katmanında başka bir 12 hizmet oluşturabilirsiniz. Katmanlar hakkında daha fazla bilgi için bkz. [Azure Search için BIR SKU veya katman seçme](../articles/search/search-sku-tier.md).

İstek üzerine en fazla hizmet sınırı oluşturulabilir. Aynı abonelik içinde daha fazla hizmete ihtiyacınız varsa Azure desteği 'ne başvurun.

| Resource            | Ücretsiz<sup>1</sup> | Temel | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| En yüksek hizmet    |1\.     | 16    | 16  | 8  | 6  | 6     | 6  | 6  |
| Arama birimlerinde maksimum ölçek (SU)<sup>2</sup> |Yok |3 SU |36 SU |36 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> boş, paylaşılan, adanmış olmayan kaynaklara dayalıdır. Paylaşılan kaynaklarda ölçek artırma desteklenmez.

<sup>2</sup> arama birimi, bir *çoğaltma* ya da *bölüm*olarak ayrılan faturalandırma birimleridir. Depolama, dizin oluşturma ve sorgu işlemleri için her iki kaynağa de ihtiyacınız vardır. SU hesaplamaları hakkında daha fazla bilgi edinmek için bkz. [sorgu ve Dizin iş yükleri Için ölçek kaynak düzeyleri](../articles/search/search-capacity-planning.md). 