---
title: Azure Veri Fabrikası varlıklarını adlandırma kuralları
description: Veri Fabrikası varlıkları için adlandırma kurallarını açıklar.
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
ms.openlocfilehash: f922ada663391cf65a61f4e18bba53668f9c4a1a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419417"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Veri Fabrikası - adlandırma kuralları

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Aşağıdaki tablo, Veri Fabrikası yapıları için adlandırma kuralları sağlar.

| Adı | İsim Tekliği | Doğrulama Denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure genelinde benzersizdir. Adlar büyük/küçük harf duyarsız, yani `MyDF` ve `mydf` aynı veri fabrikasıbakın. |<ul><li>Her veri fabrikası tam olarak bir Azure aboneliğine bağlıdır.</li><li>Nesne adları bir harf veya sayıyla başlamalıdır ve yalnızca harfler, sayılar ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakteri hemen önce ve ardından bir harf veya sayı olmalıdır. Kapsayıcı adlarında ardışık tirelere izin verilmez.</li><li>Ad 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlantılı Hizmetler/Veri Kümeleri/Boru Hatları |Bir veri fabrikasında benzersiz. İsimler büyük/küçük harf duyarsızdır. |<ul><li>Nesne adları bir harf, sayı veya alt alt (_) ile başlamalıdır.</li><li>Aşağıdaki karakterlere izin verilmez: ".", "+", "?", "/", "<", ">","*","%","&",""\\</li><li>Tire ("-") yalnızca bağlantılı hizmetlerin ve veri kümelerinin adlarında izin verilmez.</li></ul>  |
| Kaynak Grubu |Microsoft Azure genelinde benzersizdir. İsimler büyük/küçük harf duyarsızdır. | Daha fazla bilgi için [Azure adlandırma kuralları ve kısıtlamalarına](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming)bakın. |

## <a name="next-steps"></a>Sonraki adımlar
Quickstart'ta adım adım yönergeleri izleyerek veri fabrikaları oluşturmayı [öğrenin: bir veri fabrikası](quickstart-create-data-factory-powershell.md) makalesi oluşturun. 
