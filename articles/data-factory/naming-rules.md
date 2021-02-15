---
title: Azure Data Factory varlıkları adlandırma kuralları
description: Data Factory varlıkların adlandırma kurallarını açıklar.
author: dcstwh
ms.author: weetok
ms.reviewer: maghan
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: f0d14760ce3e6403c9b6fe8cc7a2100aeb3f39a6
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100372918"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory adlandırma kuralları

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Aşağıdaki tabloda Data Factory yapıtlar için adlandırma kuralları verilmiştir.

| Name | Ad benzersizliği | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Veri Fabrikası | Microsoft Azure genelinde benzersiz. Adlar, büyük/küçük harfe duyarlıdır `MyDF` ve `mydf` aynı veri fabrikasına başvurur. |<ul><li>Her veri fabrikası, tam olarak bir Azure aboneliğine bağlanır.</li><li>Nesne adları bir harf veya sayı ile başlamalıdır ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakteri hemen önce ve ardından bir harf veya sayı gelmelidir. Kapsayıcı adlarında ardışık kesik çizgilerden izin verilmez.</li><li>Ad 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/veri kümeleri/işlem hatları/veri akışları | Bir veri fabrikası içinde benzersizdir. Adlar büyük/küçük harfe duyarlıdır. |<ul><li>Nesne adları bir harfle başlamalıdır.</li><li>Şu karakterlere izin verilmez: ".", "+", "?", "/", "<", ">", "*", "%", "&", ":", " \\ "</li><li>Bağlı hizmetlerin, veri akışlarının ve veri kümelerinin adlarında tireler ("-") kullanılamaz.</li></ul>  |
| Tümleştirme Çalışma Zamanı |Bir veri fabrikası içinde benzersizdir. Adlar büyük/küçük harfe duyarlıdır. |<ul><li>Tümleştirme çalışma zamanı adı yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>İlk ve son karakterlerin bir harf veya sayı olması gerekir. Her tire (-) karakteri hemen önce ve ardından bir harf veya sayı gelmelidir.</li><li>Tümleştirme çalışma zamanı adında ardışık kesik çizgilerden izin verilmez. </li></ul> |
| Veri akışı dönüşümleri | Bir veri akışı içinde benzersiz. Adlar büyük/küçük harfe duyarlıdır | <ul><li>Veri akışı dönüştürme adları yalnızca harf ve sayı içerebilir</li><li>İlk karakter harf olmalıdır. </li></ul> |
| Kaynak Grubu |Microsoft Azure genelinde benzersiz. Adlar büyük/küçük harfe duyarlıdır. | Daha fazla bilgi için bkz. [Azure adlandırma kuralları ve kısıtlamaları](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming). |

## <a name="next-steps"></a>Sonraki adımlar
[Hızlı başlangıç: Veri Fabrikası oluşturma](quickstart-create-data-factory-powershell.md) makalesinde adım adım yönergeleri izleyerek veri fabrikaları oluşturmayı öğrenin. 
