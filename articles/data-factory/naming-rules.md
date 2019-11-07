---
title: Azure Data Factory varlıkları adlandırma kuralları
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
ms.openlocfilehash: 04de8a72d248311729fbee647322679634b5c982
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684588"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory adlandırma kuralları
Aşağıdaki tabloda Data Factory yapıtlar için adlandırma kuralları verilmiştir.

| Ad | Ad benzersizliği | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure genelinde benzersiz. Adlar büyük/küçük harf duyarsızdır, diğer bir deyişle `MyDF` ve `mydf` aynı veri fabrikasına başvurur. |<ul><li>Her veri fabrikası, tam olarak bir Azure aboneliğine bağlanır.</li><li>Nesne adları bir harf veya sayı ile başlamalıdır ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakteri hemen önce ve ardından bir harf veya sayı gelmelidir. Kapsayıcı adlarında ardışık kesik çizgilerden izin verilmez.</li><li>Ad 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/veri kümeleri/işlem hatları |Bir veri fabrikasında ile benzersizdir. Adlar büyük/küçük harfe duyarlıdır. |<ul><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Şu karakterlere izin verilmez: ".", "+", "?", "/", "<", ">", "*", "%", "&", ":", "\\"</li><li>Yalnızca bağlı hizmetlerin adlarında ve yalnızca veri kümelerinde tireler ("-") kullanılamaz.</li></ul>  |
| Kaynak Grubu |Microsoft Azure genelinde benzersiz. Adlar büyük/küçük harfe duyarlıdır. | Daha fazla bilgi için bkz. [Azure adlandırma kuralları ve kısıtlamaları](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/considerations/naming-and-tagging#resource-naming). |

## <a name="next-steps"></a>Sonraki adımlar
[Hızlı başlangıç: Veri Fabrikası oluşturma](quickstart-create-data-factory-powershell.md) makalesinde adım adım yönergeleri izleyerek veri fabrikaları oluşturmayı öğrenin. 
