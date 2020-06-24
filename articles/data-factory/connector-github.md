---
title: GitHub'a Bağlan
description: Ortak veri modeli varlık başvurularınızı belirtmek için GitHub kullanın
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: daperlov
ms.openlocfilehash: 0e17580524d6a67934aed83c6f745583b92e2422
ms.sourcegitcommit: 99d016949595c818fdee920754618d22ffa1cd49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84771045"
---
# <a name="use-github-to-read-common-data-model-entity-references"></a>Ortak veri modeli varlık başvurularını okumak için GitHub kullanın

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

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