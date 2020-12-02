---
title: Azure Data Factory varlıkları adlandırma kuralları-sürüm 1
description: Data Factory v1 varlıklarının adlandırma kurallarını açıklar.
services: data-factory
documentationcenter: ''
author: dcstwh
ms.author: weetok
manager: jroth
ms.reviewer: maghan
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 0ef997f54deafc56f1b3af603071c0e650be40d4
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96495404"
---
# <a name="rules-for-naming-azure-data-factory-entities"></a>Azure Data Factory varlıkları adlandırma kuralları
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız, bkz. [Data Factory kuralları adlandırma](../naming-rules.md).

Aşağıdaki tabloda Data Factory yapıtlar için adlandırma kuralları verilmiştir.

| Ad | Ad benzersizliği | Doğrulama denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure genelinde benzersiz. Adlar, büyük/küçük harfe duyarlıdır `MyDF` ve `mydf` aynı veri fabrikasına başvurur. |<ul><li>Her veri fabrikası, tam olarak bir Azure aboneliğine bağlanır.</li><li>Nesne adları bir harf veya sayı ile başlamalıdır ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakteri hemen önce ve ardından bir harf veya sayı gelmelidir. Kapsayıcı adlarında ardışık kesik çizgilerden izin verilmez.</li><li>Ad 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/tablolar/işlem hatları |Bir veri fabrikasında ile benzersizdir. Adlar büyük/küçük harfe duyarlıdır. |<ul><li>Bir tablo adındaki en fazla karakter sayısı: 260.</li><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Şu karakterlere izin verilmez: ".", "+", "?", "/", "<", ">", "*", "%", "&", ":", " \\ "</li></ul> |
| Kaynak Grubu |Microsoft Azure genelinde benzersiz. Adlar büyük/küçük harfe duyarlıdır. |<ul><li>En fazla karakter sayısı: 1000.</li><li>Ad harf, rakam ve şu karakterleri içerebilir: "-", "_", "," ve "."</li></ul> |

