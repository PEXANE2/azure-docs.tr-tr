---
title: GitHub'a Bağlan
description: Ortak veri modeli varlık başvurularınızı belirtmek için GitHub kullanın
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: daperlov
ms.openlocfilehash: 3ad666a477595b8367b388c4343df8aaed561a87
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84435446"
---
# <a name="use-github-to-read-common-data-model-entity-references"></a>Ortak veri modeli varlık başvurularını okumak için GitHub kullanın

Azure Data Factory 'daki GitHub Bağlayıcısı, yalnızca eşleme veri akışı 'nda [ortak veri modeli](format-common-data-model.md) için varlık başvurusu şemasını almak için kullanılır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

GitHub bağlantılı hizmeti için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği **GitHub**olarak ayarlanmalıdır. | evet
| userName | GitHub Kullanıcı adı | evet |
| password | GitHub parolası | evet |

## <a name="next-steps"></a>Sonraki Adımlar

Eşleme veri akışında bir [kaynak veri kümesi](data-flow-source.md) oluşturun.