---
title: Azure Data Factory varlıkları adlandırma kuralları | Microsoft Docs
description: Data Factory varlıkların adlandırma kurallarını açıklar.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/16/2018
ms.openlocfilehash: c31cffd3c1734e8f71f8971d597eb9e3703ae331
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72166436"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory adlandırma kuralları
Aşağıdaki tabloda Data Factory yapıtlar için adlandırma kuralları verilmiştir.

| Ad | Ad benzersizliği | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure genelinde benzersiz. Adlar büyük/küçük harf duyarsızdır, diğer bir deyişle `MyDF` ve `mydf` aynı veri fabrikasına başvurur. |<ul><li>Her veri fabrikası, tam olarak bir Azure aboneliğine bağlanır.</li><li>Nesne adları bir harf veya sayı ile başlamalıdır ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakteri hemen önce ve ardından bir harf veya sayı gelmelidir. Kapsayıcı adlarında ardışık kesik çizgilerden izin verilmez.</li><li>Ad 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/veri kümeleri/işlem hatları |Bir veri fabrikasında ile benzersizdir. Adlar büyük/küçük harfe duyarlıdır. |<ul><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Şu karakterlere izin verilmez: ".", "+", "?", "/", "<", ">", "*", "%", "&", ":", "\\"</li><li>Yalnızca bağlı hizmetlerin adlarında ve yalnızca veri kümelerinde tireler ("-") kullanılamaz.</li></ul>  |
| Kaynak grubu |Microsoft Azure genelinde benzersiz. Adlar büyük/küçük harfe duyarlıdır. | Daha fazla bilgi için bkz. [Azure adlandırma kuralları ve kısıtlamaları](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/considerations/naming-and-tagging#resource-naming). |

## <a name="next-steps"></a>Sonraki adımlar
[Hızlı başlangıç: Veri Fabrikası oluşturma](quickstart-create-data-factory-powershell.md) makalesinde adım adım yönergeleri izleyerek veri fabrikaları oluşturmayı öğrenin. 
