---
title: Azure Veri Fabrikası varlıklarını adlandırma kuralları
description: Veri Fabrikası varlıkları için adlandırma kurallarını açıklar.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 8fa1340b586434bf98d51437d4dc6b08594f0afa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931882"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Veri Fabrikası - adlandırma kuralları
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki adlandırma kurallarına](../naming-rules.md)bakın.

Aşağıdaki tablo, Veri Fabrikası yapıları için adlandırma kuralları sağlar.

| Adı | İsim Tekliği | Doğrulama Denetimleri |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure genelinde benzersizdir. Adlar büyük/küçük harf duyarsız, yani `MyDF` ve `mydf` aynı veri fabrikasıbakın. |<ul><li>Her veri fabrikası tam olarak bir Azure aboneliğine bağlıdır.</li><li>Nesne adları bir harf veya sayıyla başlamalıdır ve yalnızca harfler, sayılar ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakteri hemen önce ve ardından bir harf veya sayı olmalıdır. Kapsayıcı adlarında ardışık tirelere izin verilmez.</li><li>Ad 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlantılı Hizmetler/Tablolar/Boru Hatları |Bir veri fabrikasında benzersiz. İsimler büyük/küçük harf duyarsızdır. |<ul><li>Tablo adındaki maksimum karakter sayısı: 260.</li><li>Nesne adları bir harf, sayı veya alt alt (_) ile başlamalıdır.</li><li>Aşağıdaki karakterlere izin verilmez: ".", "+", "?", "/", "<", ">","*","%","&",""\\</li></ul> |
| Kaynak Grubu |Microsoft Azure genelinde benzersizdir. İsimler büyük/küçük harf duyarsızdır. |<ul><li>Maksimum karakter sayısı: 1000.</li><li>Ad harfleri, rakamları ve aşağıdaki karakterleri içerebilir: "-", "_", "" ve "."</li></ul> |

