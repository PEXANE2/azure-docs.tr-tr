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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80272960"
---
Bir abonelik içinde birden çok hizmet oluşturabilirsiniz. Her biri, belirli bir katmanda sağlanabilir. Yalnızca her katmanda izin verilen hizmet sayısıyla sınırlı olursunuz. Örneğin, temel katmanda 12 ' ye kadar hizmet ve aynı abonelik içindeki S1 katmanında başka bir 12 hizmet oluşturabilirsiniz. Katmanlar hakkında daha fazla bilgi için bkz. [Azure bilişsel arama için BIR SKU veya katman seçme](../articles/search/search-sku-tier.md).

İstek üzerine en fazla hizmet sınırı oluşturulabilir. Aynı abonelik içinde daha fazla hizmete ihtiyacınız varsa Azure desteği 'ne başvurun.

| Kaynak            | Ücretsiz<sup>1</sup> | Temel | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| En yüksek hizmet    |1     | 16    | 16  | 8  | 6  | 6     | 6  | 6  |
| Arama birimlerinde maksimum ölçek (SU)<sup>2</sup> |Yok |3 SU |36 SU |36 SU |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> boş, paylaşılan, adanmış olmayan kaynaklara dayalıdır. Paylaşılan kaynaklarda ölçek artırma desteklenmez.

<sup>2</sup> arama birimi, bir *çoğaltma* ya da *bölüm*olarak ayrılan faturalandırma birimleridir. Depolama, dizin oluşturma ve sorgu işlemleri için her iki kaynağa de ihtiyacınız vardır. SU hesaplamaları hakkında daha fazla bilgi edinmek için bkz. [sorgu ve Dizin iş yükleri Için ölçek kaynak düzeyleri](../articles/search/search-capacity-planning.md). 